<?php
    //GET CSRF TOKEN
    $ch = curl_init("https://profile.callofduty.com/cod/login");
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_COOKIEJAR, 'cookie.txt');
    
    $repsonse = curl_exec($ch);
            
    $dom = new DOMDocument;
    libxml_use_internal_errors(true);
    $dom -> loadHTML($repsonse);
    libxml_use_internal_errors(false);
    $tags = $dom->getElementsByTagName('meta');
    for ($i = 0; $i < $tags->length; $i++) {
        $grab = $tags->item($i);
        if($grab->getAttribute('name') === '_csrf') {
            $token = $grab->getAttribute('content');
        }
    }
    
    
    setcookie('new_SiteId', 'cod');
    setcookie('ACT_SSO_LOCALE', 'en_US');
    setcookie('country', 'US');
    setcookie('XSRF-TOKEN', $token);
    
    //LOGIN DATA
    $email = "email";
    $passoword = "passowrd";
    
    ///GENERATE MD5 DEVICE ID
    $deviceId = md5(uniqid(rand(), true));
    
    
    ///START CALL FOR AUTH TOKENS                                                                   
    $data = array('deviceId' => $deviceId);                                                                    
    $data_string = json_encode($data);                                                                                   
    $ch = curl_init('https://profile.callofduty.com/cod/mapp/registerDevice');   
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");                                                                     
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string);                                                                  
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);  
    curl_setopt($ch, CURLOPT_COOKIESESSION, true );
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true );
    curl_setopt($ch, CURLOPT_COOKIEFILE, 'C:\xampp2\htdocs\codmw\cookies.txt');
    curl_setopt($ch, CURLOPT_COOKIEJAR, 'C:\xampp2\htdocs\codmw\cookies.txt');
    curl_setopt($ch, CURLOPT_HTTPHEADER, array(                                                                          
        'Content-Type: application/json',                                                                                
        'Content-Length: ' . strlen($data_string))                                                                       
    );         
    curl_exec($ch);

    $result = json_decode(curl_exec($ch), true);
    $authHeader = $result["data"]["authHeader"];
    $auth = "Authorization: Bearer $authHeader";
    $device = "x_cod_device_id: $deviceId";
  
    //SEND AUTH HEADERS $auth AND $device AND LOGIN DATA
    $data = array('email' => $email, 'password' => $passoword); 
    $data_string3 = json_encode($data);  
    curl_setopt($ch, CURLOPT_URL, 'https://profile.callofduty.com/cod/mapp/login');                                                               
    curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");  
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true );
    curl_setopt($ch, CURLOPT_POSTFIELDS, $data_string3);  
    curl_setopt($ch, CURLOPT_COOKIEFILE, 'C:\xampp2\htdocs\codmw\cookies.txt');
    curl_setopt($ch, CURLOPT_COOKIEJAR, 'C:\xampp2\htdocs\codmw\cookies.txt');
    curl_setopt( $ch, CURLOPT_COOKIESESSION, true );
    curl_setopt($ch, CURLOPT_HTTPHEADER, array(                                                                          
        'Content-Type: application/json', 
        $auth,
        $device,
        'Content-Length: ' . strlen($data_string3))                                                                       
    );                                                                                                                   
    curl_exec($ch);
    $result = json_decode(curl_exec($ch), true);
    
    
    //RECIEVE 3 COOKIES
    $rtkn = $result["rtkn"];
    $ACTSSO = $result["s_ACT_SSO_COOKIE"];
    $atkn = $result["atkn"];
    
    
    //  Initiate curl with the 3 cookies
    $username = "Username";
    $url = "https://my.callofduty.com/api/papi-client/stats/cod/v1/title/mw/platform/psn/gamer/$username/profile/type/mp"; // EDIT FOR YOUR OWN NEEDS
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_URL,$url);
    curl_setopt($ch, CURLOPT_HTTPHEADER, ["Cookie: rtkn=$rtkn;ACT_SSO_COOKIE=$ACTSSO;atkn=$atkn"]);
    $result=curl_exec($ch);
    curl_close($ch);

    var_dump(json_decode($result, true));
    ?>
