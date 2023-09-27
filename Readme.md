# КМБ по изготовлению печатных плат и пайке

## Предисловие

Хотелось бы сразу прояснить несколько вещей. Данный гайд по большей части предназначен для людей, которые никогда в своей жизни платы не делали, реальных схемотехнических решений не придумывали, а паяльник видели только на картинке. однако, возможно для базированных инжирнеров-сантехников тоже что-то найдется, т.к планирую все это описать в мелких деталях, постепенно передавая свой десятилетний опыт в этом непростом деле. Ну что ж fellow вмсисяне и народ с ки (леший его знает как вас называть), поехали!

## Софт

Первая проблема, которая перед нами возникает - это выбор софта для трассировки плат. Тут рекомендовать что-то не буду, так как все же дело вкуса, однако перечислю
наиболее популярные решения:

- **SprintLayout6** - древняя софтина, мой flavour of choice, имеет отвратный автотрассировщик, но в ручном режиме с ним можно вытворять страшные вещи. Легко достается на первом попавшемся трекере, уже крякнутый и с кучей макросов на все случаи жизни.
- **EasyEDA** - китайский браузерный кад, имеет свою специфику работы. Так как это не просто трассировщик, а целый кад, то имеет функционал зарисовки схем и даже 3D модель платы может выдать. Также имеет фактически бесконечную библиотеку элементов, которые китайцы тщательно отмеряли и зарисовали. Автотрасса, как по мне там отличная, но не прижился у меня из-за борьбы с UI.
- **DipTrace** - профессиональное десктопное решение, умеет все то, что умеет легкая еда или даже больше, сам не пробовал, но ходят слухи, что те кто его пробуют на землю больше не возвращаются.
- **Eagle** - поделка от Autodesk, будь они прокляты, вроде идет вместе с Fusion360, также как и предыдущие варианты является кадом.

## Трассировка

Тут будут перечислены небольшие советы по самой разводке плат и схемотехнике в частности, по большей части советы валидны для всякой цифровой CMOS дряни:

- При разводке платы нужно заранее убедиться в габаритах самой платы.
- Если вы что-то намудрили и кажется, что выхода из лабиринта нет, используйте перемычки на обратную сторону платы.
- В случае изготовления многослойных плат рекомендую выносить питание и землю на отдельные крупные полигоны на своих слоях, что во первых улучшит экранирование платы, за счет большой площади данных полигонов, во вторых - облегчит разводку в целом.
- Любые импульсные или ВЧ генераторы и преобразователи необходимо разносить как можно дальше от цифровой части и сигнальных линий, ибо наведенный импульсный мусор никто не отменял, а отлаживать проблемы такого рода крайне тяжело.
- По питанию всех интегральных схем и повышаек/понижаек необходимо устанавливать керамические конденсаторы развязки, величина данных конденсаторов часто описывается в датшите. Однако есть простое правило, для цифровой CMOS логики обычно хватает 100nF.
- Замкнутые кольцевые шины вокруг логики тоже лучше не делать.
- Если используете какой-либо интерфейс где нужна подтяжка по питанию, на пуллапы контроллера лучше не надеяться, разведите внешние, схема от этого не особо подоражает.
- Интерфейсные линии нужно делать как можно короче, если конечно интерфейс не использвует диф. пару, особенно при использовании 3.3В логики.
- Убедитесь в том, что все логические уровни в вашей схеме согласованы по датшитам, оно может и так заработать, но к сдаче курсача, как на зло, что-то да выгорит.
- Внешнее тактирование нужно располагать как можно ближе к потребителю, чтобы потом не рыдать из-за того, что кварц не заводится.
- Если корпус микросхемы предполагает пайку теплоотвода (sop-8 например), то его тоже нужно развести, иначе возможно несоблюдение терморежима со всеми вытекающими. Цены устройству это не добавит, ибо фольгированный текстолит не просто так называется.
- Если у микросхемы несколько пар входов питания, то нужно подвести их все, и каждую развязать конденсатором. Некоторые подводят питание на одну пару и с другой пытаются подключить нагрузку, а потом с потерянным видом смотрят, как их дорогущий контроллер выпускает белый дым.
- Не нужно спешить с разводкой платы, банальную человескую невнимательность никто не отменял.
- Если не планируете использовать паяльную маску, не жалейте меди на места пайки разъемов, фольга там отрывается и перегревается очень легко.
- Проверяйте в своем софте, выдержит ли ваша шина ток нагрузки, иначе выйдет оригинальная гирлянда.
- Если ваша схема имеет высоковольтную часть и нужна обратная связь с низковольтной, развяжите входы логики через оптопары.
- Обращайте внимание на типоразмеры элементов, двухватный DIP резистор в место для пайки резистора на 0.25 Вт так просто впихнуть не выйдет.
- Обращайте внимание на нагрузочную способность выходов микросхем, ибо просадка может загубить вам все старания.
- Не надо надеяться на то, что ваши аппаратные косяки поправятся софтом, скорее все вы, или человек, пишущий софт, просто положите болт на проблему. На крайний случай может возникнуть костыльное реешение, которое, займет половину ROM'а вашего контроллера, а все из-за того, что кто-то забыл поставить RC-фильтр.

Для кого-то эти советы могут показаться очевидными, для кого-то бредовыми, однако, прошу заметить, что ваш слуга прошел этот путь потом, кровью и парой сотен убитых электронных приборов и интегральных схем. Даже профессионалы порой игнорирут основы, что приводит к выходу из строя или неопределенному поведению даже очень крутых и дорогих девайсов.

## Перенос рисунка и процесс изготовления

Процесс переноса рисунка всегда осуществляется по одной технологии, если совсем кратко, напечатал, вырезал, приложил к плате, проутюжил.
Однако и тут кроются свои подводные камни. Начнем с начала.

### Предварительные приготовления и исходные материалы

Тут перечислю почти все что нам потребуется.

- Лазерный принтер. Тонер, который использует конкретный принтер может сильно повлиять на результат переноса, не у всех есть время и деньги проверять кучу принтеров, могу порекомендовать Canon LBP6030, картридж принтера по возможности не должен быть перезаправлен, ибо полосы и просветы. Полосы и просветы есть всегда в случае использования б/у картриджей, просто они не всегда видимы глазу людскому.
- Глянцевая бумага для переноса рисунка. Народ использует всякое, многие рекомендуют фотобумагу Lomond, факсовую бумагу, старые глянцевые журналы, однако ваш слуга остановился на желтой термотрансферной бумаге с алика, перегреть ее **невозможно**, в отличии от вышеописанных вариантов, а большинство проблем как раз возникают по причине недогрева или перегрева бумаги.
- Фольгированный стеклотекстолит. Бывает односторонним и двухсторонним. Он будет основой для нашей платы.
- Банка комета. Это нам нужно для отчистки фольгированного текстолита от оксидов, да и к тому же он имеет хорошие абразивные свойства.
- Любой углеводордный растворитель. Нужен для очистки от жирных разводов.
- Утюг, желательно с максимально чистой и плоской рабочей поверхностью.
- Старая зубная щетка. Ей будем текстолит чистить от всякой дряни
- Кастрюля. Нужна для водяной бани.
- Конетейнер для химии. Нужен для едкого раствора, чтоб не запоганить кастрюлю.
- Едкая химия. Про едкую химию подробно опишу в следующем разделе.

### Едкая химия

Едкая химия нужна для того, чтоб растворить открытые участки меди на текстолите. Конкретных пропорций давать не буду, ибо у всех все работает по разному. Можно использовать всякую гадость различной вредности и едкости:

- Хлорное железо. Воняет моргом и оставляет дыры везде где попадет, включая пальцы. Травит платы быстро, даже при малых объемах раствора. А самое главное, что его можно использовать много раз, моей банке с ним уже лет 7.
- Перекись + Азотная кислота. Еще более термоядерная гадость воняет потными носками, которые неделю держали в скисшем молоке. При реакции выделяет бурый газ, который из ваших легких сделает тяжелые, а из глаз - рыбьи. Как люди этим травят платы, не понимаю, однако, травит быстро и без подогрева.
- Перекись + Лимонная кислота + щипотка соли. Народный вариант, работает хорошо но нужен подогрев, травит минут 15. Раствор одноразовый.
- Персульфат Натрия/Амония. Дорогущий вариант, травит быстро, требует подогрева. Раствор одноразовый, дырки на одежде тоже обеспечивает.
- Медный купорос. Самый плохой из всех вариантов. Травит долго и болезненно 4-5 часов, если вообще вытравит.

Выбирайте, что понравилось.

### Техпроцесс

Пришло время подробно описать техпроцесс:

1. Очистка текстолита. Зачастую текстолит покрыт всякими жирными пятнами и оксидами, нам нужно от них избавиться, для лучшей адгезии тонера. Выполняем грубую чистку зубной щеткой и кометом, текстолит нужно смочить проточной водой, при этом комет должен быть смочен так, чтоб его гранулы еще хрустели на плате. Это нужно для небольшого абразивного эффекта. **Тереть наждачкой текстолит без резкой необходимости не надо, так как это создаст кучу царапин на его поверхности, там тонер не прилипнет!** В общем трем до блестящей поверхности без черных пятен и разводов, попутно добавляя воды и комета. После грубой чистки необходимо еще раз хорошо промыть текстолит под проточной водой, а затем протреть растворителем. Как только протерли растворителем, нужно тут же бумажным полотенцем или туалетной бумагой высушить текстолит от него, так как если оставить его самостоятельно испаряться, будут разводы. **С этого момента текстолит пальцами не лапаем, брать можно только за края!**
2. Печать шаблона. Печатаем на глянцевой бумаге шаблоны несколько раз, это нужно для того чтобы сразу было несколько попыток на всякий пожарный случай. **Стоит обратить внимание на то, что после переноса на текстолит шаблон будет зеркально отражен!** Шаблон печатаем на глянцевой стороне, если бумага односторонняя.
3. Перенос шаблона. После печати желательно тут же начать процесс переноса, так как по опыту, шаблоны, полежавшие минут 30-40 переводятся гораздо хуже. Вырезаем ножницами шаблон, если на бумаге осталось чистое место, можно ее не выкидывать, а использовать повторно. Далее необходимо на ровной поверхности положить тестолит, прижать к нему шаблон стороной с напечатанным и хорошенько пригреть горячим утюгом. После того, как шаблон прилип к тесктолиту, нужно хорошо пройтись краем утюга по всей площади платы. В случае термотрансферной бумаги, весь рисунок будет видно через нее, если все было сделано правильно. **Если используете иной тип бумаги то возможно потребуется прослойка ткани или обычной бумаги между утюгом и платой, иначе перегрев и капут!**
4. Удаление бумаги. После прогрева и переноса, даем плате остыть, как только ее можно будет брать пальцами тут же смачиваем ее проточной водой. Некоторые типы бумаги сами отлипать не захотят, и нужно пальцами скатать ее с платы. В случае термотрансферной бумаги достаточно подцепить ее за край ногтем и она тут же полностью отойдет.
5. Проверка результата. Если результат удовлетворителен, то можно идти дальше, иначе стираем тонер растворителем, и повторяем с первого пункта. **Под удовлетворителным результатом будем понимать полный или почти полный перенос рисунка!**
6. Исправление косяков. В случае неполного переноса рисунка, можно с помощью зубочистки и лака для ногтей попытаться довести нужные дорожки.
7. Подготовка к травлению. Можно травить на холодную, однако, время экспозиции сильно вырастет, что может привести подъеданию меди под защищенными участками на плате. Поэтому будем использовать водяную баню. Набираем воды в кастрюлю и ставим на газ. Едкую химию заливаем в контейнер для травления, залить нужно так, чтоб плата полностью утонула в химии. Контейнер можно использовать пищевой из пластика, однако баланс у них довольно плохой, и нужно следить за тем чтоб все не перевернулось. Если плата очень большая, то имеет смысл подогреть раствор заранее в **неметаллической** посуде на водяной бане, а позже перелить в более подходящий по размеру контейнер. **Банки из-под оугрцов лучше не использовать, если треснет то нужно с кастрюлей выкинуть вместе!** Хорошо бы обзавестись химической посудой из боросиликатного стекла. Если травите прям на бане, то плату можно закинуть сразу в контейнер с химией и греть все это до появления видимого пара из растора для для травления, попутно проверяя плату. Как только пар пошел из раствора, то можно снимать его с бани. Далее покачиваем контейнер и проверяем плату.
Плата готова, когда на ней не видно открытых следов меди, т.е остались только наши дорожки. Доставать плату из контейнера можно палочками для еды или пинцетом, если вам его не жалко. Воду можно довести до кипения и приготовить в ней пельмени XDD (**Лучше не надо**).
8. Очистка платы. Достаем плату из контейнера и кидаем под проточную воду, а раствор назад в банку или в слив. После промывки под краном протираем плату растворителем, для удаления тонера.

Если все сделано правильно, то процесс суммарно обычно не занимает более 45 минут.

### Постпроцессинг

До непосредственно пайки нужно еще сделать пару мелочей.

1. Провеслить все отверстия для перемычек и DIP компонетов.
2. Залудить все плату припоем. Это нужно для устранения микротрещин в меди текстолита и для защиты от коррозии. Процесс простой, покрываем всю плату флюсом и пропаиваем все дорожки и контактные площадки. Про флюсы и пайку написано в следующем разделе.

## Пайка

В данном разделе ваш слуга поведает вам о сокральных тайнах пайки, о типах оборудования для пайки и демонтажа, о припоях и флюсах.

### Оборудование и способы пайки

Как показала практика паять на самом деле можно чем угодно, главное чтоб оно было достаточно горячим. На практике используют следующие способы:

- Пайка жалом паяльника. Самый очевидный способ пайки, на нем зацикливаться не будем.
- Пайка горячим воздухом. Для пайки горячим воздухом используют специальный фен. Фены бывают двух типов: компрессорный и турбинный. По характеру и силе воздушного потока они ведут себя очень по разному. Этот способ хорошо подходит для демонтажа и монтажа всякой мелко и среднегабаритной поверхностной гадости.
- Пайка нижним подогревом. Данный способ хорош для плат, где используютсякомпоненты поверхностного монтажа. Я встречал индивидов, которые паяли так платы в духовке.
- Инфрокрасная пайка. По сути способ пайки лампой ильича. Способ подходит для монтажа и демонтажа крупногабаритных теплоемких поверностных элементов на свинцовых шарах, обычно после демонтажа компонента возникает необходимость в повторной накатке шаров по трафарету. Может возникнуть вопрос, а почему бы фен не использовать? На этот вопрос ответит вам печатная плата видеокарты, которую после пайки феном повело и сморщило и теперь она ремонту не подлежит. Если сама плата очень теплоемкая, то зачстую процесс ИК пайки совмещают с нижним подогревом.

Вспомогательное оборудование для монтажа и демонтажа:

- Оловоотсос. Лучший друг человека, удалит все лишние сопли, с помощью вакуума. Оловоотсосы бывают, механические( с ручним механизмом взведения) и автоматические (компрессорные), также можно выделить отсосы с подогревом и без, в случае если отсос без подогрева, необходимо расплавить припой наиболее удобным для способом, а затем удалить излишки прпоя.
- Пинцет. Трогать горячие компоненты пальцами идея так себе, поэтому народ придумал данный девайс. Формы пинцетов описывать не буду, опять же дело вкуса и зависит от конкретной задачи.
- Мультиметр. Он нужен для проверки наличия электрического контакта в месте соединения. В данном случае используется в режиме диодной прозвонки. **Цифровые Мультиметры бывают без нормальной кнопки включения и без функции автоматического отключения питания, так что не забываем вернуть переключатель в стандартное положение, иначе элемент питания отдаст богу душу!**

### Типы паяльников и уход за паяльником

На практике все юзабельные паяльники являются электрическими, есть и газовые, но эту адскую поделку в расчет брать не будем.

Электрические паяльники можно класифицировать несколько категорий:

- По типу нагревательного элемента. Обычно это нихромовый нагреватель или керамический. Предпочитаю керамику.
- По потребляемой мощности. Очевидно, что слабым паяльником паять крупногабаритный теплоемкий хлам будет не очень.
- По типу управления температурой жала. Бывают цифровые, аналоговые и неуправляемые. Предпочтительно использовать паяльник с цифровым управлением.
- По напряжению питания и наличию гальванической развязки в цепи питания паяльника. Если паять всякую чувствительную аппаратуру обычным кипятильником с рынка, то скорее всего у вас в схеме что-то помрет после пайки. Для решения данной проблемы используют изолированные от сети низковольтные паяльники с хорошим заземлением.

Далее опишу типы жал и способ ухода за ними:

- Медные жала. Дедовский вариант. Было время когда я паял SMD рассыпуху дедовским сороколетним паяльником, накрутив на него толстую медную проволоку. Медные жала сильно обгорают со временем и для их чистки обычно используют напильник. После очистки до блеска необходимо залудить жало, для этого холодный паяльник опускаем в канифоль, вкючаем паяльник в сеть и ждем как только канифоль закипит, затем хорошенько втираем паяльник в припой. Про процесс лужения расскажу чуть позже.
- Никилированные жала. Данный тип жал готов к пайке всегда, однако необходимо периодически выполнять чистку.**Тереть данные жала напильником, идея крайне плохая! Также данные жала очень чувствительны к температурному режиму и любять деформироваться при перегреве.** Чистка данных жал выполняется с помощью влажной целлюлозной губки, смочить губку можно водой или глицерином. Горячий паяльник быстрыми движениям очищаем о влажную губку.

**Тут небольшая ремарка по формам жала, если жало тонкое, то оно для тонкой работы и точка.**

### Припои

Припои классифицируют по тонне разных параметров, но нас интересуют следующие:

- Температура плавления. Данный параметр всегда указывают в описании.
- Наличие канала флюса. Если припой имеет канал флюса, то им паять гораздо удобнее.
- Форм фактор. Припой продается в разных форматах, нам интересен припой в виде проволоки.

Рекомендуемые припои:

 - Отечественный ПОС61;
 - Китайские припои 63/37.

 Данные припои имеют низкую температуру плавления, обычно для них хватает 250-300 градусов на паяльнике, в зависимости от теплоемкости жала. Чистое олово, сплав розе и прочие фокусы заморские не рекомендую, по причине их спонтанного саморазрушения при перепадах влажности и температуры. Соотношение или число в названии говорит о содержании Олова и Свинца в припое в процентном соотношении.

### Флюсы

Флюс - это специальная химия, которая нужна для удаления оксидов с деталей перед пайкой, иначе припой просто не смочит поверхность металла. Флюсы можно разделить на две крупные категории:

- Активные флюсы. Данные флюсы продолжают разъедать металл независимо от температурного режима. Требуют полной отмывки.
- Пассивные флюсы. Такие флюсы активируются только при воздействии высокой температуры. Отмывка опциональна, однако, рекомендую отмывать любой флюс.

Отмывку флюсов можно производить с помощью растворителя, проточной воды и ультразвуковой ванны. Тип Жидкости для отмывки зависит от состава флюса. **Некторые флюсы хорошо проводят ток, что может повлиять на работу схемы!**

Флюсы бывают:

- твердыми;
- гелиобразными;
- жидкими.

Рекомендуемые флюсы:

- Канифоль сосновая.
- Канифоль на спирте(СКФ).
- ЛТИ-120. Тот же СКФ, но с активаторами, требует отмывки.
- NC-559. Китайский SMD гель, мой основной флюс.

Можно заметить, что прям едкой дряни типо паяльной кислоты в списке нет. Для пайки электроники вообще лучше такое не использовать, угробите свои платы и жала.

## Процесс пайки

Для того чтоб спаять два проводника, необходимо нанести на каждый из них флюс, а затем паяльником нанести тонкий слой припоя по всей площади контакта. Наненесение тонкого слоя припоя на проводник называют лужением. Я предпочитаю лудить скошенным круглым жалом. После лужения, бОльшим количеством припоя можно спаять детали, излишки припоя лучше удалить с места соединения, если таковые образовались. Температуру паяльника подбираем так, чтобы припой был блестящим и текучим, в таком случае поверхностное натяжение сделает почти всю работу за вас. После пайки не забываем отмыть флюс. Чем быстрее вы выполните пайку, тем лучше будет для вашей платы и ее компонентов.

Далее будет небольшой траблшутинг по пайке:

- В случае если дорожки на плате слиплись, плавим на месте нежелательного контакта кусочек канифоли, или пытаемся удалить излишки отсосом.
- Залитые отверстия тоже можно пробивать отсосом, если такового нет под рукой, то подойдет вывод от любого пассивного компонета или иная более-менее жесткая проволока.
- Перетравленные или выгоревшие участки дорожек на плате можно соединить проволочкой или повесить соплю припоя.
- В случае если на плате были разведены лишние дорожки, или часть разводки дефектная, можно их аккуратно перерезать концелярским ножом.
- На мелких дорожках или соединениях не всегда виден контакт или его отсутствие, обязательно звоним тестером.
- При пайке микросхем, обрайте внимание на ключи.
- Мелкие чипы легче паять феном, прихватываете пару выводов паяльником, наносите флюс, и прогреваете феном до усадки. Далее по необходимости пропаиваем выводы паяльником.
- При демонтаже электронных приборов не нужно прилагать чрезменое усилие, иначе повредите сам прибор и плату.

## Фидбэк

Если вы обнаружили ошибки, очепятки, иные неточности формулировки, или хотите просто дополнить данный материал, форкните репозиторий, внесите свои правки и откройте пр. В названии укажите тему, а в описании причину внесения правки.




