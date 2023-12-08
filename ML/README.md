## Описание проекта
Заказчик производит несколько сотен различных товаров бытовой и промышленной химии, а затем продаёт эти товары через дилеров. Дилеры, в свою очередь, занимаются розничной продажей товаров в крупных сетях магазинов и на онлайн площадках. Для оценки ситуации, управления ценами и бизнесом в целом, заказчик периодически собирает информацию о том, как дилеры продают их товар. Для этого они парсят сайты дилеров, а затем сопоставляют товары и цены. Зачастую описание товаров на сайтах дилеров отличаются от того описания, что даёт заказчик. Например, могут добавляться новый слова (“универсальный”, “эффективный”), объём (0.6 л -> 600 мл). Поэтому сопоставление товаров дилеров с товарами производителя делается вручную.

**Цель этого проекта** - разработка решения, которое отчасти автоматизирует процесс сопоставления товаров. Основная идея - предлагать несколько товаров заказчика, которые с наибольшей вероятностью соответствуют размечаемому товару дилера. Предлагается реализовать это решение, как онлайн сервис, открываемый в веб-браузере. Выбор наиболее вероятных подсказок делается методами машинного обучения.

**Критерий успеха** - метрика accuracy@n при n в интервале от 1 до 10, то есть нужный товар попал в первые n в предложенном списке.

## Распределение задач

Дмитрий - разработка и оптимизация основной модели с использованием *BERT*.

Наталья - предобработка, разработка модели на основе *tf_idf+knn*, отчет.

Денис - руководство, предобработка, разработка модели на основе *SentenceTransformers*.

## Что было сделано в процессе работы

После загрузки и первоначального осмотра данных стало понятно, что конкретная задача сводится к подобору для каждого товара дилера *marketing_dealerprice['product_name']* несколько наиболее похожих на него товаров из списка производителя *marketing_product[name]*. Проверить правильность соответствия можно было с помощью отдельной таблицы *marketing_productdealerkey*. 

Данные содержали слова смешанного регистра на двух языках (русском и английском), иногда не разделенные пробелами, и различные знаки препинания, не всегда уместно расставленные. Также встречались двойные пробелы. Предположение "очистка данных повысит метрику" подтвердилось. 

Вариативность имелась в двух местах:
- как векторизировать;
- как оценивать близость.

В качестве оценки близости было решено использовать косинусную меру, так как она она широко распространена в рекомендательных системах и в работе с текстами, а также, как показали эксперименты, повышала метрику.

Для векторизации члены команды предложили несколько вариантов решений - от относительно простых *tf_idf+knn* до использования предобученных нейросетей. Далее каждый занялся реализацией своих предложений. Общей была предобработка и идея: условный *fit* происходит на названиях производителя *marketing_product['name']*(около 500 строк), а подбор осуществляется для названий дилеров *marketing_dealerprice['product_name']*(около 1600 строк после удаления дубликатов и пропусков).

Было разработано 3 модели. Все модели принимали на вход очищенные данные.
Самая простая - *tf_idf+knn* векторизировала дополнительно лемматизированные названия с использованием биграмм, после чего посредством *sklearn.neighbors.NearestNeighbors* искала ближайших соседей входящего названия дилера среди названий производителя.
Другая модель использовала эмбеддинги, полученные предобученным *SentenceTransformer('distiluse-base-multilingual-cased-v1')* для поиска наиболее похожих названий. Наконец, третья модель получала эмбеддинги с помощью *BertModel.from_pretrained'cointegrated/LaBSE-en-ru'*.

Так как задача несколько нестандартная в рамках классического машинного обучения, разделения данных на тренировочную и валидационную выборки не требовалось. Валидация моделей происходила по всей выборке товаров дилеров.

## Какие метрики качества модели использовали

Для оценки качества моделей использовалась метрика accuracy@n при n в интервале от 1 до 10. В таблице представлена эта метрика для различных n, полученная разработанными моделями.

|n|*tf_idf+knn*|*SentenceTransformer*|*BertModel*|
|-|-|-|-|
|1|0.697|0.692|0.762|
|3|0.877|0.849|0.910|
|5|0.917|0.895|0.940|
|10|0.960|0.930|0.967|

Лучший результат показала *BertModel*. В решении реализована она.

## Используемые библиотеки:

numpy, pandas, torch, transformers, scipy

## Краткая инструкция, как воспользоваться решением

Для применения решения нужно вызвать функцию *result(marketing_product_csv, marketing_dealerprice_csv, quantity_int)*, где *quantity_int* - желаемое количество подбираемых товаров. Функция возвращает *json*-словарь, где каждому товару из *marketing_dealerprice_csv* соответствует список артикулов товаров из *marketing_product_csv*.