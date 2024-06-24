<h1 align="center"> <ins> Национальный исследовательский университет "МИЭТ" </ins> </h1>

<h1 align="center"> Экзаменационный билет №5 </h1>

<h2 align="center"> по курсу «Теория цифровой обработки сигналов для телекоммуникационных систем» </h2>

1. Изменение частоты дискретизации сигнала. Фильтр Farrow.

Многие системы цифровой обработки сигналов имеют в своих схемах более 1 частоты дискретизации сигнала, меняющейся на протяжении обработки сигнала. Такое решение может быть применено из-за ряда причин, например:
- для уменьшения вычислительной сложности;
- для облегчений требований к аналоговым и цифровым фильтрам;
- в случае, если алгоритмы ЦОС работают на разных частотах дискретизации;
- или же для требований синхронизации.

Для перехода от одной частоты дискретизации сигнала к другой необходимы специальные техники, одна из которых и будет рассмотрена ниже. 

Прямым примером необходимости преобразования частоты дискретизации сигнала является случай использования ЦАП. При работе на наименьшей возможной частоте дискретизации по Найквисту понадобится аналоговый фильтр со слишком крутой АЦХ, что будет означать значительное увеличение сложности схемотехники и потребляемого тока. На рисунке 1 видно, что использование более высокой частоты дискретизации для ЦАПа позволит использовать аналоговый фильтр с более пологой АЦХ. Отсюда следует стремление повысить частоту дискретизации сигнала перед ЦАП.

Ещё одним примером необходимости изменения частоты дискретизации являются цифровые приемники, внутри которых используется множество частот дискретизации. АЦП дискретизирует сигнал с частотой, значительно превышающей скорость передачи данных, заявленную в системе. После этого используется преобразователь понижения частоты дискретизации сигнала до промежуточной частоты, которая уже ближе к фактической скорости обновления сигнала. Наиболее эффективный способ сделать это - использовать цепочку фильтров с понижением частоты дискретизации, где каждый фильтр уменьшает частоту дискретизации на целочисленный коэффициент. В конце этой цепочки частота дискретизации сигнала будет равна фактической частоте обновления сигнала. 

Логичен вопрос, почему бы сразу не дискретизировать сигнал с нужной полосой? Ответ в том, что в этом случае перед АЦП понадобится чрезвычайно высокопроизводительный аналоговый фильтр, защищающий нас от эффекта наложения частот в спектре. Для ряда систем связи разработать фильтр, который справлялся бы с такой задачей, фактически невозможно. 

Перейдем к рассмотрению возможных операций над частотой дискретизации сигнала. Возможны следующие действия: 
- понижение частоты дискретизации
- повышение частоты дискретизации
- изменение частоты дискретизации в целочисленный коэффициент раз
- изменение частоты дискретизации в дробный коэффициент раз

*Следует помнить, что для изменения частоты дискретизации практически всегда необходима фильтрация сигнала!*

Понижение частоты дискретизации сигнала происходит путем выполнения децимации - процедуры отбрасывания ненужных отсчётов сигнала, перед которой необходима предварительная фильтрация, реализующая ограничение полосы сигнала, для защиты от появления в спектре выходного сигнала ложных гармоник. 

Повышение частоты дискретизации сигнала происходит путем расширения сигнала средствами вставки нужного числа отсчётов между исходными отсчётами сигнала, после которой необходима фильтрация, реализующая подавление зеркальных частот. 

*Изменение частоты дискретизации сигнала на нецелочисленный коэффициент всегда значительно сложнее.*

На рисунке 2 приведен пример повышения частоты дискретизации сигнала в 4 раза. 

Математически взаимосвязь между отсчётами выходного $y[n]$ и входного $x[n]$ сигналов при повышении дискретизации сигнала в $N$ раз выражается следующим образом: 

$y[n]=x[{n \over N}]$, если $n = \lambda N$, где $\lambda$ - целое число, или

$y[n]=0$, если $n \ne \lambda N$.

Другими словами, $y[n]=x[{n/N}]$ для случаев, когда $n$ без остатка делится на $N$, в противном случае $y[n]=0$.

Таким образом, выходная частота дискретизации сигнала $f_u$ ставновится равна $Nf_s$, где $f_s$ - исходная частота дискретизации сигнала. 

На рисунке 3 приведено изменение спектра сигнала после повышения частоты дискретизации. На нём видно, что спектральные копии появились в до частоты дискретизации $f_u/2$. Эти спектральные копии необходимо удалить фильтром нижних частот. 

На рисунке 4 видно, что фильтр нижних частот должен удалить все копии спектра сигнала в диапазоне частот от $f_s/2$ до $f_u/2$. Такой фильтр нижних частот называется интерполирующим. 

Рисунок 5 отражает, почему такой фильтр называется интерполирующим. На этом рисунке представлен сигнал во временной области. В данном примере проводилось повышение частоты дискретизации сигнала в 2 раза. Фильтрация копий спектра сигнала во временной области эквивалентна сглаживанию выборок сигнала во временной области, в результате чего происходит интерполяция. 

Заметим, что использование фильтра в такой схеме абсолютно неэффективно. При повышении частоты дискретизации сигнала в $N$ через фильтр проходит ровно $N-1$ нулей, которые тратят ресурсы умножителей. Существует техника декомпозиции фильтра на полифазные компоненты, благодаря которой повышается эффективность использования элементов схемы. Схема такой архитектуры фильтра представлена на рисунке 6. 

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

Фильтр Фарроу реализует эти формулы выполняя сначала множение $t = Ax$, соответствующее $(M+1)$ операции фильтрации в каждом отводе общего КИХ фильтра.

2. Прием сигналов с OFDM и синхронизация. Типовая блок-схема приемника OFDM. Чувствительность к сдвигу по частоте. Точная синхронизация, отслеживание по пилотным поднесущим. Способы выбора пилотных поднесущих.

