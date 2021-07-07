# Отправка сообщений из формы на сайте в Telegram

Если у вас возникнут вопросы по настройки данной интеграции, то вы всегда можете обратиться ко мне за помощью заполни форму на моём сайте - https://prog-time.ru/telegram_integration/

## Настраиваем Telegram (создаём бота)

И так для натсройки нам понадобится авторизоваться в телеграмм и 

Для того чтобы отправить письмо в телеграмм, нам необходимо сделать несколько последовательных действий.
1) Вам нужно авторизоваться в Telegram аккаунте
2) В поиске найти пользователя @BotFather
3) Отправить сообщение боту — /newbot
4) После отправки запроса , нужно указать имя бота (на английском языке)
5) После этого дублировать название бота, но только суффиксом _bot
6) После успешной регистрации бота, @BotFather пришлёт вам сообщение с токеном, который вам нужно сохранить, в дальнейшем он нам понадобится.
7) Теперь нам нужно создать чат в который мы добавим нашего бота
8) Далее нам нужно получить id нашего бота. Для этого нужно перейти по следующей ссылке, где за место символов X нужно подставить ваш токен:
https://api.telegram.org/botXXXXXXXXXXXXXXXXXX/getUpdates
Не закрывайте эту страницу, после 9 пункта, её нужно будет обновить.
9) Теперь вам необходимо отправить команду /join в чат для активации бота. После отправки команды, вам нужно обновить страницу, чтобы сделать повторный запрос.
10) Здесь вам нужно записать следующий фрагмент кода — id вашего бота. Эту информацию тоже нужно записать.
Вам нужен id со знаком минус.
<pre>
"my_chat_member":{"chat":{"id":-594377170, ...
</pre>

Теперь вам нужно создать форму обратной связи и отправить полученные данные по следующему URL:
https://api.telegram.org/botваш_токен/sendMessage?chat_id=идентификаатор_чата&text=текст_сообщения

Для отправки запроса, воспользуемся библиотекой Curl. Информацию по использованию данной библиотеке, вы можете получить в следующей записи — https://prog-time.ru/parsing-php-biblioteka-curl/
  
  
## Создаём форму
  
### HTML
  
Для примера я создал самую простую форму из 3 полей и кнопки. В качестве обработчика использую файл sendTelegram.php
  
### PHP
  
Здесь прописываем код обработчика нашей формы
  
<pre>
/*получаем значения полей из формы*/
$name = $_POST['name'];
$phone = $_POST['phone'];
$email = $_POST['email'];

/*функция для создания запроса на сервер Telegram */
function parser($url){
    $curl = curl_init();
    curl_setopt($curl, CURLOPT_URL, $url);
    curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
    $result = curl_exec($curl);
    if($result == false){     
      echo "Ошибка отправки запроса: " . curl_error($curl);
      return false;
    }
    else{
      return true;
    }
}

/*собираем сообщение*/
$message .= "Новое сообщение из формы";
$message .= "Имя: ".$name;
$message .= "Телефон:".$phone;
$message .= "Email:".$email;

/*токен который выдаётся при регистрации бота */
$token = "1724061264:AAHTshrcn33459712fdsf2zM-bDviT-QgF0tAM";
/*идентификатор группы*/
$chat_id = "-594377134470";
/*делаем запрос и отправляем сообщение*/
parser("https://api.telegram.org/bot{$token}/sendMessage?chat_id={$chat_id}&parse_mode=html&text={$message}");
</pre>
  
  
  ## Для удобства я использую функцию
  
  
<pre> 
/* ОТПРАВКА ПИСЬМА В TELEGRAM */
/*функция для создания запроса на сервер Telegram */
function parser($url){
    $curl = curl_init();
    curl_setopt($curl, CURLOPT_URL, $url);
    curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
    $result = curl_exec($curl);
    if($result == false){     
      echo "Ошибка отправки запроса: " . curl_error($curl);
      return false;
    }
    else{
      return true;
    }
}

function orderSendTelegram($message) {
    $message = urlencode($message);

    /*токен который выдаётся при регистрации бота */
    $token = "1616052093:AAGZ5vsKRTT92jj2yHaZ34534fvZ4tZIUB_8N-tY";
    /*идентификатор группы*/
    $chat_id = "-644535881116";

    /*делаем запрос*/
    parser("https://api.telegram.org/bot{$token}/sendMessage?chat_id={$chat_id}&parse_mode=html&text={$message}");
}

</pre>
  
  
  

