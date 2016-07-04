---
title:   Дата и час
isChild: true
---

## Дата и час

В PHP има клас на име DateTime, който помага при четенето, писането и сравняването и изчисляването на дата и час.
Има много фунцкии за работа с дата и час в PHP, но DateTime специално предлага хубав обектно-ориентиран интерфейс за
работа с дата и час. Има възможност за работа с часови пояси, но това е отвъд въведенитео.

За да започнете работата с DateTime, преобразувайте дата и час от низ (string) в обект чрез метода `createFromFormat()`. За да започнете от текущото време, използвайте `new \DateTime`. С метода `format()` можете да преобразувате DateTime обект обратно в низ за изход.
{% highlight php %}
<?php
$raw = '22. 11. 1968';
$start = \DateTime::createFromFormat('d. m. Y', $raw);

echo 'Start date: ' . $start->format('m/d/Y') . "\n";
{% endhighlight %}

Изчисленията с дати и часове се правят чрез класа DateInterval. Методите `add()` и `sub()` на класа DateTime приемат DateInterval за аргумент. Не пишете код, който очаква определен брой секунди всеки ден, смятата на зимно и лятно време, катко и промяната
на часовия пояс може да доведе до грешка. Използвайте интервали от време вместо това. За да изчислите разликата между две дати,
използвайте `diff()` метода. Той ще върне нов DateInterval обект, който е много лесе нза показване.
{% highlight php %}
<?php
// create a copy of $start and add one month and 6 days
$end = clone $start;
$end->add(new \DateInterval('P1M6D'));

$diff = $end->diff($start);
echo 'Difference: ' . $diff->format('%m month, %d days (total: %a days)') . "\n";
// Difference: 1 month, 6 days (total: 37 days)
{% endhighlight %}

На обекти от тип DateTime можеш да прилагаш обикновено сравненеие:
{% highlight php %}
<?php
if ($start < $end) {
    echo "Start is before end!\n";
}
{% endhighlight %}

Един последен пример ще демонстрира класа DatePeriod. Той се използва за обхождане на повтарящи се събития.
Приема два DateTime обекта - начало и край, и интервал, и връща всички обекти по средата.
{% highlight php %}
<?php
// output all thursdays between $start and $end
$periodInterval = \DateInterval::createFromDateString('first thursday');
$periodIterator = new \DatePeriod($start, $periodInterval, $end, \DatePeriod::EXCLUDE_START_DATE);
foreach ($periodIterator as $date) {
    // output each date in the period
    echo $date->format('m/d/Y') . ' ';
}
{% endhighlight %}

* [Прочети относно DateTime][datetime]
* [Прочети относно форматирането][dateformat] (опциите, които форматирането приема)

[datetime]: http://www.php.net/manual/book.datetime.php
[dateformat]: http://www.php.net/manual/function.date.php
