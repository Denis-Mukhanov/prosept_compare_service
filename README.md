<div style="padding: 30px 25px; border: 2px #6495ed solid">
    
[Реализация ML-решения](https://prosept.streamlit.app/)

__Хакатон PROSEPT:__ 1-е место в номинации "Лучшая работа DS"

__Заказчик:__ [ООО «ПРОСЕПТ»](https://prosept.ru/) — российская производственная компания, специализирующаяся на выпуске профессиональной химии. Производство и логистический центр расположены в непосредственной близости от Санкт-Петербурга.

__Описание проекта:__ Заказчик производит несколько сотен различных товаров, а затем продаёт эти товары через дилеров. Дилеры, в свою очередь, занимаются розничной продажей товаров в крупных сетях магазинов и на онлайн площадках. Для оценки ситуации, управления ценами и бизнесом в целом, заказчик периодически собирает информацию о том, как дилеры продают их товар. Для этого они парсят сайты дилеров, а затем сопоставляют товары и цены. Зачастую описание товаров на сайтах дилеров отличаются от того описания, что даёт заказчик. Например, могут добавляться новый слова (“универсальный”, “эффективный”), объём (0.6 л -> 600 мл). Поэтому сопоставление товаров дилеров с товарами производителя делается вручную.

__Цель проекта:__ Разработка решения, на основе [технического задания](https://github.com/Denis-Mukhanov/prosept_compare_service/tree/main/data/TZ_prosept.pdf) которое отчасти автоматизирует процесс сопоставления товаров. Основная идея - предлагать несколько товаров заказчика, которые с наибольшей вероятностью соответствуют размечаемому товару дилера. Предлагается реализовать это решение, как онлайн сервис, открываемый в веб- браузере. Выбор наиболее вероятных подсказок делается методами машинного обучения.

__Основные методы, примененные при реализации проекта:__
- Streamlit
- NLTK
- TfidfVectorizer
- BERT
- SentenceTransformer

__Результаты:__
В рамках проекта был разработан ML-продукт, автоматизирующих процесс сопоставления товаров, в частности:

- Проанализированы данные, проведен инжиниринг признаков и подготовка их для обучения модели,
- Настроены и обучины модели, выбрана оптимальная и оценино качество прогноза,
- Реализована сборка и финализирован ML-продукт для запуска в продуктовое использование.
    
    
</div>
