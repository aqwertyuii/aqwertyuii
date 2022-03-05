<?php
$token = 5214713959:AAG0Z2Nb_QcslL7y5llb5sf3ZZErOfSdFX0"5140826481:AAG0Z2Nb_QcslL7y5llb5sf3ZZErOfSdFX0
define('API_KEY',$token);
echo file_get_contents("https://api.telegram.org/bot" . API_KEY . "/setwebhook?url=" . $_SERVER['SERVER_NAME'] . "" . $_SERVER['SCRIPT_NAME']);
function bot($method,$datas=[]){
$url = "https://api.telegram.org/bot".API_KEY."/".$method;
$ch = curl_init();
curl_setopt($ch,CURLOPT_URL,$url);
curl_setopt($ch,CURLOPT_RETURNTRANSFER,true);
curl_setopt($ch,CURLOPT_POSTFIELDS,$datas);
$res = curl_exec($ch);
if(curl_error($ch)){
var_dump(curl_error($ch));
}else{
return json_decode($res);
}
  
}
$update = json_decode(file_get_contents('php://input'));
$message = $update->message;
$chat_id = $message->chat->id;
$text = $message->text;
$from_id = $message->from->id;
$data = $update->callback_query->data;
$chat_id2 = $update->callback_query->message->chat->id;
$message_id2 = $update->callback_query->message->message_id;
#/////////(hmo)////////#
if ($text == "/start") {
  bot('sendmessage', [
    'chat_id'=>$chat_id,
    'text'=>"اهلا عزيزي في بوت تحميل من تيك توك/n• ارسل رابطك ليتم تنزيل",
    'reply_markup'=>json_encode([
      'inline_keyboard'=>[
        [['text'=>"المطور" ,'url'=>"t.me//vvvpd"]]
        ]
      ])
    ]);
}
#/////////(hmo)////////#
if(preg_match("/(.*?)tiktok(.*?)/",$text)){
  bot('sendphoto',[
    'chat_id'=>$chat_id,
    'photo'=>$text,
    'caption'=>"اختر الذي تريد ارساله لك",
    'reply_markup'=>json_encode([
      'inline_keyboard'=>[
        [['text'=>"فيديو" ,'callback_data'=>"/ve $text"],
         ['text'=>"mp3" ,'callback_data'=>"/audio $text"]],
        ]
      ])
    ]);
}
#/////////(hmo)////////#
if(preg_match('/ve /', $data )){
  $ve = explode('/ve ',$data)[1];
$api =json_decode(file_get_contents('https://iiraq.tk/api/tik.php?url='.$ve));
bot('Sendvideo',[
'chat_id'=>$chat_id2,
'message_id'=>$message_id2,
'video'=>$api->video[0],
]);
}
#/////////(hmo)////////#
if(preg_match('/audio /', $data )){
  $audio = explode('/audio ',$data)[1];
$api =json_decode(file_get_contents('https://iiraq.tk/api/tik.php?url='.$audio));
  bot('Sendaudio',[
'chat_id'=>$chat_id2,
'message_id'=>$message_id2,
'audio'=>$api->music[0],
