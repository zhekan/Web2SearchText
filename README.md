# Web2SearchText
The program find text in web pages and all url in it

# Використані бабліотеки
- Qt - використовувалася для побудови графічного інтерфейсу та розпаралелювання
- Curl - використовувалася для отримання html коду за вказаною url адресою

# Граф посилань
Так як програмі не потрібно знати всіх зв'язків в графі url посилань,
(зв'язки які утворюють замкнуті цикли не потрібні) то було прийнято 
рішення використовувати дерево з довільною кількістью дочірніх вузлів.

Як і було поставлено в задачі, використовувався обхід в ширину.
При обході використовувався метод двох черг, поточної і наступної.
При вивільнені першої до них застосовувався метод std::swap.

Обхід був адаптований для багатопоточного виконання, для кожної
з списку поточного нод застосовувався метод scan_node який передавався
в QThreadPool з використанням QFutureSynchronized. І програма очікувала 
допоки усі scan_node для current_level не виконаються. після чого запускала обробку next_level.

# Графічний інтерфейс
Для побудови графічного інтерфейсу був вибраний дизайн головного вікна з вкладками:
- Setting, де задаються початкові умови;
- Findlist - cgbcjr url посилань де було найдено збіг;
- Log - вивід повідомлень в процесі виконання

Так як закачка і сканування відбуваються надзвичайно швидко я вирішив відображати процес у формі лога
де відображався вже результат сканування (знайдено/не знайдено/помилки приспробі завантажити html)

# Пропозиції щодо тестування
- Провести модульне тестування в першу чергу функцій які працюють з вхідними даними, тобто
  ті які завантажують та обробляють html сторінки (файл url_utils.h).
- Також потребує детальної уваги цикл який сробить обхід дерева з подальшим скануванням. Тут
цого можна просто виконувати з різною кількістю потоків для різної кількості просканованих
посилань
- Провести Інтеграційне тестування всієї системи в цілому
