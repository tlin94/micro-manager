<?php


if ($_FILES["file1"]["error"] > 0)
  {
    echo "Error: " . $_FILES["file1"]["error"] . "<br>";
  }
else if ($_FILES["file2"]["error"] > 0)
  {
	echo "Error: " . $_FILES["file2"]["error"] . "<br>";
  } 
else 
  {
  echo "Material Input File : <br>"	 ;
  echo "Upload: " . $_FILES["file1"]["name"] . "<br>";
  echo "Type: " . $_FILES["file1"]["type"] . "<br>";
  echo "Size: " . ($_FILES["file1"]["size"] / 1024) . " kB<br><br>";
  
  echo "Dislocation Input File : <br>"	;
  echo "Upload: " . $_FILES["file2"]["name"] . "<br>";
  echo "Type: " . $_FILES["file2"]["type"] . "<br>";
  echo "Size: " . ($_FILES["file2"]["size"] / 1024) . " kB<br><br>";
  
  $textMaterial = base64_encode(file_get_contents($_FILES["file1"]["tmp_name"])); 
  $textDislocation = base64_encode(file_get_contents($_FILES["file2"]["tmp_name"])); 
  $nProcessors =  $_POST["number"];
  $xml = '<data>
		<Material>'.$textMaterial.'</Material>
		<Dislocation>'.$textDislocation.'</Dislocation>
		<FunctionName>StartSimulation</FunctionName>
		<NumofProcessors>'.$nProcessors.'</NumofProcessors>
		</data>
		';     
	 
	
	//Send the HTTP request
	$url = "http://localhost:8080/receiveXML.html";
		 
	$ch = curl_init($url);
	//curl_setopt($ch, CURLOPT_MUTE, 1);
	curl_setopt($ch, CURLOPT_POST, 1);
	curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: text/xml'));
	curl_setopt($ch, CURLOPT_POSTFIELDS, "$xml");
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	$output = curl_exec($ch);
	echo $output;
    curl_close($ch);
		
  
  
  }   
?> 

