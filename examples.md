# Примеры кода
[Как массово обновить большой объем данных в Laravel, чтобы не нагружать базу данных](#massbdupdate)

### [Как массово обновить большой объем данных в Laravel, чтобы не нагружать базу данных](#massbdupdate)
[Bulk Update Multiple Records with Separate Data — Laravel](https://medium.com/@sentiasa/bulk-update-multiple-records-with-separate-data-laravel-3da9131c279a)
```
<?php

$vat = 0.20;
$transactions = Transaction::get();

foreach ($transactions->chunk(5000) as $chunk) {
   $cases = [];
   $ids = [];
   $params = [];
   
   foreach ($chunk as $transaction) {
       $cases[] = "WHEN {$transaction->id} then ?";
       $params[] = $transaction->profit * $vat;
       $ids[] = $transaction->id;
   }

   $ids = implode(',', $ids);
   $cases = implode(' ', $cases);

   if (!empty($ids)) {
       \DB::update("UPDATE transactions SET `price` = CASE `id` {$cases} END WHERE `id` in ({$ids})", $params);
   }
}
```
