---
title: PHP_curl 远程抓取图片
date: 2018-05-20 15:54:23
tags:
---
## 远程图片抓取。
```
function GrabImage($url,$filename="") {
    if($url==""):return false;endif;

    if($filename=="") {
        $ext=strrchr($url,".");
        if($ext!=".gif" && $ext!=".jpg"):return false;endif;
        $filename=date("dMYHis").$ext;
    }
/*
    $img = file_get_contents($url);
    if(!$img){
    	return false;
    }
    file_put_contents($filename,$img);
*/
    $ch=curl_init();
    $timeout=5;
    curl_setopt($ch,CURLOPT_URL,$url);
    curl_setopt($ch,CURLOPT_RETURNTRANSFER,1);
    curl_setopt($ch,CURLOPT_CONNECTTIMEOUT,$timeout);
    $img=curl_exec($ch);
    curl_close($ch);

//    ob_start();
//    $file = readfile($url);
//    $img = ob_get_contents();
//    ob_end_clean();
//    $size = strlen($img);
    if(!$img) return false;

    $fp2=@fopen($filename, "a");
    fwrite($fp2,$img);
    fclose($fp2);

    return $filename;
}
```

- 如果没有配https
D:\work\sites\img_p2peye\upload.php 70行中https变成http
```

echo json_encode ( array (
		"code" => 200,
		"url" => "https://img.p2peye.com{$path}/$res",
		"file" => $path . "/" .$res,
		"name" => $res,
		"file_name" => $path . '/'
) );
```
变成
```

echo json_encode ( array (
		"code" => 200,
		"url" => "http://img.p2peye.com{$path}/$res",
		"file" => $path . "/" .$res,
		"name" => $res,
		"file_name" => $path . '/'
) );
```