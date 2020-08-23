<?php
//データまとめ
    $name=$_POST["name"];
    $comment=$_POST["comment"];
    $time=date("Y/m/d H:i:s");
    $pass=$_POST["pass"];  
    $Dpass=$_POST["Dpass"];
    $Epass=$_POST["Epass"];
    $filename="3-5.txt";
    
//送信ボタン動作    
    if(isset($_POST["Ssubmit"]) && empty($_POST["lognumber"]) && !empty($_POST["pass"])){
        if(file_exists($filename)){
            $num=count(file($filename))+1;
        }else{
            $num=1;
        }  
        $data=$num."<>".$name."<>".$comment."<>".$time."<>".$pass."<>".PHP_EOL;
        if(!empty($_POST["name"])||!empty($_POST["str"])){
            $fp = fopen($filename,"a");
            fwrite($fp,$data);
            fclose($fp);
        }
    }

//削除ボタン動作
    if(isset($_POST["Dsubmit"])){
        $Dnum=$_POST["delete"];
        $Dlines=file($filename,FILE_IGNORE_NEW_LINES);
        $Dfp=fopen($filename,"w");
            foreach($Dlines as $Dline){
                $Dlog=explode("<>",$Dline);
                    
                if($Dlog[0]==$Dnum && $Dlog[4]==$Dpass){
                    $Ddata1="0<>削除済み".PHP_EOL;
                    fwrite($Dfp,$Ddata1);
                }else{
                    $Ddata2=$Dlog[0]."<>".$Dlog[1]."<>".$Dlog[2]."<>".$Dlog[3]."<>".$Dlog[4]."<>".PHP_EOL;
                    fwrite($Dfp,$Ddata2);
                }
            }
            fclose($Dfp);
    }
    
//編集機能準備
    if(isset($_POST["Esubmit"])){
        $Enum=$_POST["edit"];
        $Elines=file($filename,FILE_IGNORE_NEW_LINES);
            foreach($Elines as $Eline){
            $Elog=explode("<>",$Eline);
            
//投稿番号で編集するデータなのか認識            
                if($Elog[0]==$Enum && $Elog[4]==$Epass){
                    $Elognum=$Elog[0];
                    $Ename=$Elog[1];
                    $Ecomment=$Elog[2];
                    $Epassword=$Elog[4];
                }    
            }
    }
    if(isset($_POST["Ssubmit"]) && isset($_POST["lognumber"]) && !empty($_POST["pass"])){
        $Enum2=$_POST["lognumber"];
        $Elines2=file($filename,FILE_IGNORE_NEW_LINES);
        $Efp=fopen($filename,"w");
            foreach($Elines2 as $Eline2){
                $Elog2=explode("<>",$Eline2);
                    
                if($Elog2[0]==$Enum2 && $Elog2[4]=$pass){
                    $Edata1=$Enum2."<>".$name."<>".$comment."<>".$time."<>".$pass."<>".PHP_EOL;
                    fwrite($Efp,$Edata1);
                }else{
                    $Edata2=$Elog2[0]."<>".$Elog2[1]."<>".$Elog2[2]."<>".$Elog2[3]."<>".$Elog2[4]."<>".PHP_EOL;
                    fwrite($Efp,$Edata2);
                }
            }
            fclose($Efp);
        
    }

    ?>
<!DOCTYPE html>
<html lang="ja">
    <head>
        <meta charset="UTF-8">
        <title>mission_3-5</title>
    </head>
<body>
    <div align="left"><h1 class="midashi_1">趣味</h1></div>
    
        <form action="mission_3-5.php" method="post">
            <input type="txt" name="name" size=5 placeholder="名前" value="<?php echo $Ename; ?>">
            <input type="txt" name="comment" placeholder="コメント" value="<?php echo $Ecomment; ?>">
            <input type="txt" name="pass" size=5 placeholder="パスワード" value="<?php echo $Epassword; ?>">
            <input type="submit" name="Ssubmit">
            <input type="hidden" name="lognumber" placeholder="nosee" value="<?php echo $Elognum; ?>">
            <input type="txt" name="edit" size=5 placeholder="編集番号">
            <input type="txt" name="Epass" size=5 placeholder="パスワード">
            <input type="submit" name="Esubmit" value="編集">
            <input type="txt" name="delete" size=5 placeholder="削除番号">
            <input type="txt" name=Dpass size=5 placeholder="パスワード">
            <input type="submit" name="Dsubmit" value="削除">
        </form>
</body>
</html>
<?php
//画面表示
    if(file_exists($filename)){
        $lines=file($filename,FILE_IGNORE_NEW_LINES);
        foreach($lines as $line){
        $log=explode("<>",$line);
            if($log[0]==0){
                echo "";
            }elseif($log[0]!=0){
            echo$log[0]." ".$log[1]." ".$log[2]." ".$log[3]."<br>";
            }
        }             
    }
    
    
    
    ?>
