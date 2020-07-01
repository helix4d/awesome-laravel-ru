# Примеры кода

### Bulk Update Multiple Records with Separate Data — Laravel
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
