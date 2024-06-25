<h1 align="center"> <ins> Национальный исследовательский университет "МИЭТ" </ins> </h1>

<h1 align="center"> Экзаменационный билет №5 </h1>

<h2 align="center"> по курсу «Теория цифровой обработки сигналов для телекоммуникационных систем» </h2>

<h3 align="center"> 1. Изменение частоты дискретизации сигнала. Фильтр Farrow. </h3>

Основной источник: 
- https://cloud.mail.ru/public/LLXm/NDvfyMj1r/MultirateSignalProcessing.pdf

Многие системы цифровой обработки сигналов имеют в своих схемах более 1 частоты дискретизации сигнала, меняющейся на протяжении обработки сигнала. Такое решение может быть применено из-за ряда причин, например:
- для уменьшения вычислительной сложности;
- для облегчений требований к аналоговым и цифровым фильтрам;
- в случае, если алгоритмы ЦОС работают на разных частотах дискретизации;
- или же для требований синхронизации.

Для перехода от одной частоты дискретизации сигнала к другой необходимы специальные техники, одна из которых и будет рассмотрена ниже. 

Простым примером необходимости преобразования частоты дискретизации сигнала является использование Ц-А преобразователя и фильтра после него. При работе ЦАП на наименьшей возможной частоте дискретизации (по Найквисту или Котельникову) понадобится аналоговый фильтр со слишком крутой АЦХ, что будет означать значительное осложнение схемотехники и увеличение потребляемого тока. На рисунке 1 видно, что использование более высокой частоты дискретизации для ЦАПа позволит использовать аналоговый фильтр с более пологой АЦХ. Отсюда следует стремление повысить частоту дискретизации сигнала перед ЦАП.

<div align="center">
    <figure >
    <img src="/pics/reconstruction_filter.drawio.svg" width="99%"/>
    <figcaption><center>Рисунок 1 - Сравнение требуемых АЧХ фильтров для разных частот дискретизации.</center></figcaption>
    </figure>
</div>

Ещё одним примером необходимости изменения частоты дискретизации являются цифровые приемники, внутри которых используется множество частот дискретизации. АЦП дискретизирует сигнал с частотой, значительно превышающей скорость передачи данных, заявленную в системе. Фильтрация сигнала на выходе АЦП должна происходить на частоте дисретизации, значительно превышающей реальную частоту изменения сигнала для того, чтобы ослабить требования к фильтру, как это показано на рисунке 2. После этого используется преобразователь понижения частоты дискретизации сигнала до промежуточной частоты, которая уже ближе к фактической скорости обновления сигнала. Наиболее эффективный способ сделать это - использовать цепочку фильтров с понижением частоты дискретизации, где каждый фильтр уменьшает частоту дискретизации на целочисленный коэффициент. В конце этой цепочки частота дискретизации сигнала будет равна фактической частоте обновления сигнала. 

<div align="center">
    <figure >
    <img src="/pics/digital_downconversion.drawio.svg" width="99%"/>
    <figcaption><center>Рисунок 2 - Схема поэтапного понижения частоты дискретизации сигнала в цифровом приемнике.</center></figcaption>
    </figure>
</div>

Логичен вопрос, почему бы сразу не дискретизировать сигнал с нужной полосой? Ответ в том, что в этом случае перед АЦП понадобится чрезвычайно высокопроизводительный аналоговый фильтр, защищающий нас от эффекта наложения частот в спектре. Для ряда систем связи разработать фильтр, который справлялся бы с такой задачей, фактически невозможно. 

Перейдем к рассмотрению возможных операций над частотой дискретизации сигнала. Возможны следующие действия: 
- понижение частоты дискретизации
- повышение частоты дискретизации
- изменение частоты дискретизации в целочисленный коэффициент раз
- изменение частоты дискретизации в дробный коэффициент раз

*Следует помнить, что для изменения частоты дискретизации практически всегда необходима фильтрация сигнала!*

Понижение частоты дискретизации сигнала происходит путем выполнения децимации - процедуры отбрасывания ненужных отсчётов сигнала, перед которой необходима предварительная фильтрация, реализующая ограничение полосы сигнала, для защиты от появления в спектре выходного сигнала ложных гармоник. 

Повышение частоты дискретизации сигнала происходит путем расширения сигнала вставкой нужного числа отсчётов между исходными отсчётами сигнала, после которой необходима фильтрация, реализующая подавление зеркальных частот. 

*Изменение частоты дискретизации сигнала на нецелочисленный коэффициент всегда значительно сложнее.*

На рисунке 3 приведен пример повышения частоты дискретизации сигнала в 3 раза. 

<div align="center">
    <figure >
    <img src="/pics/upsampling.drawio.svg" width="99%"/>
    <figcaption><center>Рисунок 3 - Пример повышения частоты дискретизации сигнала в 3 раза.</center></figcaption>
    </figure>
</div>

Математически взаимосвязь между отсчётами выходного $y[n]$ и входного $x[n]$ сигналов при повышении дискретизации сигнала в $N$ раз выражается следующим образом: 

$y[n]=x[{n \over N}]$, если $n = \lambda N$, где $\lambda$ - целое число, или

$y[n]=0$, если $n \ne \lambda N$.

Другими словами, $y[n]=x[{n/N}]$ для случаев, когда $n$ без остатка делится на $N$, в противном случае $y[n]=0$.

Таким образом, выходная частота дискретизации сигнала $f_u$ ставновится равна $Nf_s$, где $f_s$ - исходная частота дискретизации сигнала. 

На рисунке 4 приведено изменение спектра сигнала после повышения частоты дискретизации. На нём видно, что спектральные копии появились в полосе до частоты дискретизации $f_u/2$. Эти спектральные копии необходимо удалить фильтром нижних частот. 

<div align="center">
    <figure >
    <img src="/pics/expansion.drawio.svg" width="99%"/>
    <figcaption><center>Рисунок 4 - Пример расширения сигнала в 3 раза вставкой нулей и следствие этого в частотной области.</center></figcaption>
    </figure>
</div>

На рисунке 5 видно, что фильтр нижних частот должен удалить все копии спектра сигнала в диапазоне частот от $f_s/2$ до $f_u/2$. Такой фильтр нижних частот называется интерполирующим. 

<div align="center">
    <figure >
    <img src="/pics/fir_interp.drawio.svg" width="99%"/>
    <figcaption><center>Рисунок 5 - Обоснование необходимости фильтрации после вставки нулей.</center></figcaption>
    </figure>
</div>

Рисунок 6 отражает, почему такой фильтр называется интерполирующим. На этом рисунке представлен сигнал во временной области. В данном примере проводилось повышение частоты дискретизации сигнала в 3 раза. Фильтрация копий спектра сигнала во временной области эквивалентна сглаживанию выборок сигнала во временной области, в результате чего происходит интерполяция. 

<div align="center">
    <figure >
    <img src="/pics/interpolating.drawio.svg" width="100%"/>
    <figcaption><center>Рисунок 6 - Процесс интерполяции сигнала в 3 раза во временной и частотной областях.</center></figcaption>
    </figure>
</div>

Заметим, что использование фильтра в такой схеме абсолютно неэффективно. При повышении частоты дискретизации сигнала в $N$ через фильтр проходит ровно $N-1$ нулей, которые тратят ресурсы умножителей. Существует техника декомпозиции фильтра на полифазные компоненты, благодаря которой повышается эффективность использования элементов схемы. Она основана на том, что в конкретный момент времени обрабатывается только одна "фаза" сигнала - такой набор его отсчетов, которые заведомо не равны нулю и действительно будут использовать умножители и сумматоры фильтра. Эта задача несколько усложняется если эта "фаза" должна быть варьируемой и коэффициенты фильтра должны перерасчитываться в реальном времени. В результате этого каждый вес фильтра должен вычисляться как некоторая фукнция от переменной величины $\rho$. Для решения этой задачи существует подход полиномильной апрроксимации. Согласно этому подходу переменный вес каждого отвода фильтра аппроксимируется многочленом довольно малой степени (чаще не выше 3). Значения полиномов вычисляются каждый раз когда необходимо. Схема такой архитектуры фильтра представлена на рисунке 7. 

<div align="center">
    <figure >
    <img src="/pics/filter_bank.drawio.svg" width="99%"/>
    <figcaption><center>Рисунок 7 - Представление банков полифазного фильтра.</center></figcaption>
    </figure>
</div>

Оказалось, что для интерполяции наилучшим оказывается такой фильтр, импульсная характеристика которого представляется собой отсчёты функции вида $sinx \over x$. Таким образом, импульсная характеристика фильтра-интерполятора может быть разбита на $L$ секций, каждая секция будет отвечать за конкретный отсчет или группу отсчетов импульсной характеристики. При этом каждый вес аппроксимируется полиномиальной функций следующего вида: 

$$ \omega _n( \rho) = \alpha _{n,0} + \alpha _{n,1} \rho + \alpha _{n,2} \rho ^2 + \alpha _{n,M} \rho ^M $$

Коэффициенты $\alpha _{n,M}$ могут быть определены с помощью техник апрроксимации кривой. На рисунке видно, что каждый из участкой можно аппроксимировать различными функциями. Линейная даст посредственное качество итоговой фукнции $sinx \over x$, квадратичная терпимое, а функция третьего порядка сделает это с более чем достаточной точностью для большинства прикладных задач. То есть каждый участок импульсной характеристики = отвод в КИХ фильтре-интерполяторе, будет аппроксимироваться своим полиномом 3 порядка. В результате чего получается, что веса КИХ фильтра-интерполятора будут представлять собой следующий вектор: 

$$ \omega ( \rho) = [\omega _0( \rho) \space \omega _1( \rho) \space \omega _2( \rho) \space ... \space \omega _{L-1}( \rho) ]^T $$

Вектор-столбец, содержащий коэффициенты $\rho ^M$ для $m=0,1,...,M$ будет выглядеть так:

$$ r = [1 \space \rho \space \rho ^2 \space \rho ^3 \space ... \space \rho ^M]^T $$

Тогда для КИХ фильтра-интерполятора длины $L$ входной сигнал будет записан в векторной форме:

$$ x = [x[n] \space x[n-1] \space x[n-2] \space ... \space ... x[n-L+1]]^T $$

С матрицей интерполирующих коэффициентов ***А*** размера $(M+1) \space * \space L$, где 

$$ A_{nm} = \alpha _{n, m} $$

Выход фильтра может быть кратко записан следующим образом: 

$$ y[k] = r^T A x $$

Фильтр Фарроу реализует эти формулы выполняя сначала умножение $t = Ax$, соответствующее $(M+1)$ операции фильтрации в каждом отводе общего КИХ фильтра. Строки матрицы $A$ с индексами $m$ обозначены через $a_m$, где $m=0, \space 1, \space ..., \space M$. Выходы отводов фильтров соединены цепочкой множителей и сумматоров. Структурная схема фильтра Фарроу представлена на рисунке 8. 

<div align="center">
    <figure >
    <img src="/pics/farrow.drawio.svg" width="99%"/>
    <figcaption><center>Рисунок 8 - Структурная схема фильтра Фарроу.</center></figcaption>
    </figure>
</div>

<h3 align="center"> 2. Прием сигналов с OFDM и синхронизация. Типовая блок-схема приемника OFDM. Чувствительность к сдвигу по частоте. Точная синхронизация, отслеживание по пилотным поднесущим. Способы выбора пилотных поднесущих. </h3>

Основой источник: 
- https://cloud.mail.ru/public/LLXm/NDvfyMj1r/OFDM_synchronisation.pdf

Типовая схема OFDM передатчика представлена на рисунке 9. На ней биты информации сначала попадают на маппер, генерирующий комплексные символы сигнального созвездия. Далее вычисляется обратное преобразование Фурье для генерации OFDM символов. После этого формируется защитный интервал между символами путем добавления циклического префикса. Далее происходит фильтрация оконной функцией, уменьшающая внеполосовые гармоники. На последнем этапе OFDM символы модулируются сначала промежуточной частотой, а затем переносятся на полосу передачи. 

Понятно, что на приемной стороне необходимо сделать то же самое, только в обратном порядке. Типовая схема OFDM приемника предствлена на рисунке 10. На рисунке также подписаны, на каких этапах какие виды синхронизации необходимо обеспечить. Очевидно, что для корректной демодуляции OFDM символа необходимо обеспечить синхронизацию по частоте несущего колебания, синхронизацию времени символа и фазовую синхронизацию поднесущих. 

***Частотная синхронизация*** включает в себя подстройку внутреннего генератора промежуточной частоты для с целью попадания в центральную частоту принятого OFDM сигнала. 

***Синхронизация по времени символа*** необходима для того, чтобы подобрать оптимальное окно, над которым проводить преобразование Фурье и демодулировать символы. В идеале период символа должен быть подобран так, чтобы амплитуда и фаза каждой поднесущей за время одного символа были постоянны. 

***Фазовая синхронизация поднесущих*** необходима для правильного принятия решения о том, какой модулированный символ был передан. Заметим, что частотная синхронизация не обеспечивает фазовую синхронизацию, а в модели канала с многолучевым распространением фаза каждой поднесущей может быть различной. Более того, для квадратурно-амплитудных модуляций помимо правильной оценки фазы необходимо оценивать и амплитуду каждой поднесущей для её коррекции и правильного решения о передаваемых символах. 

При отсутствии частотной синхронизации в принятом сигнале будет так называемый частотный сдвиг. OFDM системы гораздо более чувствительны к сдвигу по частоте чем системы с одним несущим колебанием. Говоря простым языком, частотный сдвиг уничтожает ортогональность принятых поднесущих. Более того, взятие отсчётов сигнала происходит в неоптимальные моменты времени, что в частотной области эквивалентно взятию неправильной частоты. Всё это приводит к интерференции между поднесущими. 

Частотное смещение в OFDM сродни временному смещению в цифровой системе связи с амплитудной модуляцией, где используется формирование импульсов с помощью фильтров вида $sinx \over x$ или фильтра квадратного корня из приподнятого косинуса. В такой системе выборка символа в неправильный момент времени не приводит к межсимвольной интерференции благодаря свойствам формирующих фильтров. Однако выборка в неправильный момент времени приводит к уменьшению мощности сигнала и внесению помех от соседних символов. 

В OFDM аналогичное возникает в частотной области. Прямое преобразование Фурье на приемной стороне выполняет выборку в частотной области. Если выбрана неправильная частотная точка, то ортогональность между поднесущими теряется, что приводит к появлению помех от других поднесущих. Кроме того, происходит уменьшение желаемой составляющей сигнала. 

В любой практической ситуации частоты тактовых генераторов передатчика и приемника будут отличаться, то есть будут иметь малый частотный сдвиг. Следствием этого будет являться то, что приемник будет принимать и анализировать спектр с другим масштабом и другим шагом, отличающимся от того, что было на передатчике. Пример на рисунке 11 показывает, что происходит, когда тактовый генератор на приемной стороне работает на более высокой частоте, чем частота тактового генератора передатчика. 

В качестве примера можно рассмотреть следующий сценарий. Пусть частота тактового генератора на передатчике равна $R_{tx}$ Гц, частота тактового генератора на приемнике равна $R_{rx}$ Гц. Пусть $R_{rx}$ имеет более высокую частоту чем $R_{tx}$ на $P$ ppm(parts per million). Тогда 

$$ R_{rx} = R_{tx}(1 + {P \over 10^6}) $$

Если передатчик отправляет пилотный тон частоты $f_c$ Гц, приемник видит в спектре частоту $f'_c$, где 

$$ f'_c = f_c \cdot {10^6 \over {P + 10^6}} $$

Величина частотной ошибки будет равна 

$$ f_{c, err} = f'_c \cdot {P \over 10^6} $$

В силу того, что ошибка по частоте напрямую зависит от величины самой частоты, результат будет заключаться в том, что приемник будет сканировать несколько сжатый частотный спектр. Если же частота тактового генератора на приемнике будет выше тактовой частоты в передатчике, то приемник будет обозревать несколько растянутый частотый спектр. 

Стоит сказать, что задача символьной синхронизации в OFDM хорошо известна и решается добавлением циклического префикса к передаваемому OFDM-символу. Добавленный циклический префикс используется для ведения корреляционного приема. Выход коррелятора достигает максимума на границах символов и однозначно определяет время начала OFDM-символа. 

Выход коррелятора также может быть использован для определения частотного сдвига. Заметим, что выход коррелятора $x(t)$ является комплексным сигналом. Тогда фаза $x(t)$ в месте корреляционного пика будет представлять собой разность фаз между циклическим префиксом и концом символа. В силу того, что мы знаем, что циклический префикс и конец символа отстоят друг от друга по времени на величину $T_{FFT}$, мы можем посчитать величину частотного смещения, которое привело к такой разности по времени. Таким образом величина частотного смещения составляет

$$ f_{err} = {\phi \over {2 \pi T_{FFT}}} $$

Данный метод синхронизации основан на двух предположениях: 
- Тактовые генераторы передатчика и применика работают на одной частоте;
- Характеристики канала не меняются за время передачи одного OFDM-символа.

Также заметим, что использование циклического префикса позволяет корректно обнаруживть частотное смещение лежащее только в диапазоне равном половине интервала частотного разнесения поднесущих $\pm 1/2T_{FFT}$. Для нас это выливается в измерение фазового сдвига между концом циклического префикса и концом символа в диапазоне от $-\pi$ до $\pi$ радиан. То есть фазовый сдвиг в $13 \pi / 6$ радиан будет определен как $\pi / 6$. 

Решение этой проблемы обеспечивается использованием специальных коротких обучающих символов для облегчения грубой оценки частоты. После получения грубой оценки частоты длинные информационные символы могут быть использованы для обеспечения точной оценки частоты. 

На рисунке 12 представлен пример структуры кадра OFDM системы. Некоторое число специальных коротких обучающих символов передается в начале кадра. В силу того, что они значительно короче стандартных OFDM-символов, они могут быть использованы для обнаружения гораздо большего частотного смещения. Это можно представить как процесс оцифровки частотной ошибки. Чем чаще производится выборка фазы несущей, тем выше частотная ошибка, которую можно обнаружить. Более того, все обучающие последовательности известны на приеме. Для нас это означает то, что для оценки частотного смещения могут быть использованы все данные, а не только циклический префикс. 

В реальных системых непрерывной передачи данных приемник должен уметь отслеживать отклик канала. Для этого используется техника распределения пилотных тонов по всему времени и спектру передачи, а не концентрация пилотных тонов в структуре типа преамбулы. Таким образом, накладные расходы на синхронизацию остаются низкими, а методы цифровой обработки сигналов позволяют интерполировать полученные оценки канала по всей частотно-временной сетке. 

В силу того, что пилотные тоны предоставляют нам отсчеты импульсной характеристики канала в частотной и временной областях, разнесение пилотных тонов должно удовлетворять теореме Котельникова (или Найквиста). 

***Пилотное разнесение по частоте*** $(S_f)$ должно удовлетворять условию:

$$ S_f < {1 \over \tau _{max}} $$

где $\tau _{max}$ - максимальный разброс задержки в канале.

***Пилотное разнесение по времени*** $(S_t)$ должно удовлетворять условию:

$$ S_t < {1 \over B_d} $$

где $B_d$ - допплеровский сдвиг в канале.

Полученные отсчеты импульсной характеристики канала могут интерполированы с использованием техник фильтрации-интерполяции. Рассмотрим следующий пример. Пусть есть следующая сетка частот, в которой 60 слотов для передачи данных и 6 слотов для передачи пилотного тона. Определим $p$ как вектор, состоящий из 6 оценок состояния канала, полученных из 6 пилотных тонов. Определим $h$ как вектор оценок состояний канала, полученных из 60 принятых OFDM-символов. 

$h$ может быть вычислена как линейная комбинация $p$:

$$ h = Ap $$

Матрица $A$ называется интерполирующей матрицей. Главная проблема состоит в определении этой матрицы. Она может быть получена оптимизацией минимальной средней квадратической ошибки, то есть 

$$ A = R_{hp} R_{pp}^{-1} $$

где $R_{hp}$ - матрица кросс-ковариации между векторами $h$ - реальным откликом канала на конкретный элемент данных и откликом канала на пилотный тон, а $R_{pp}$ - матрица авто-корреляции оценок канала по пилотным тонам. 

Причина, по которой ковариация участвует в этих уравнениях, заключается в том, что нам необходимо учитывать уровень корреляции между откликами канала на разных частотах и в разное время. Это определяет, какой вклад должна вносить конкретная оценка пилотного тона в каждую оценку канала.

Интерполяционная матрица может быть разработана без реальной оценки и измерений канала, на основе некоторых предположений о самом канале и уровне шума в нем. Результат такой оценки будет оптимизирован для сделанных предположений, однако останется работоспособным, если фактические характеристики канала отличаются от заданных. 

Чтобы не передавать пилотные тоны можно использовать обучающие последовательности. Обучающие последовательности состоят из тех же самых OFDM-символов, повторённых некоторое число раз. Для определения несущей частоты используется фильтр, согласованный с отсчетами обучающего символа. Коэффициенты этого фильтра представляют собой отсчеты обучающего символа, взятые по времени в обратном порядке по мере того, как он появляется на выходе обратного преобразования Фурье на передаче. Это означает, что согласованный фильтр настроен на точное соответствие передаваемой последовательности отсчетов. Прохождение принимаемого сигнала через согласованный фильтр вызывает появление коррелиционных пиков на выходе фильтра. Пик возникает тогда, и только тогда, когда весь обучающий символ находится в линии задержки фильтра и идеально синхронизирован с коэффициентами согласованного фильтра. 

Источники: 
- Richard Van Nee & Ramjee Prasad, OFDM for Wireless communiations, ISBN 0-89006-530-6, Artech House, 2000.
- T. M. Pollet, Mark Van Vladel and Marc Moeneclaey, BER Sensitivity of OFDM Systems to Carrier Frequency Offset and Wiener Phase Noise, IEEE Transactions on
Communications, Vol. 43, No 2/ 3/ 4, February/ March/ April 1995.

