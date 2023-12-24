# ed-php-generators
Unlocking the Power of PHP Generators

## As a newcomer to PHP, you might find yourself dealing with large datasets or arrays. A common challenge is the dreaded memory exhaustion error:

```
Fatal error: Allowed memory size of xxxxxx bytes exhausted (tried to allocate xxxxx bytes) in your_script.php on line xx
```

This error means your PHP script has surpassed the memory limit. To overcome this, PHP offers a neat solution called “generators,” introduced in PHP 5.5. They allow you to iterate over data sets without loading everything into memory, which is both efficient and effective.

## What are Generators?

Generators provide a way to iterate over a dataset without creating a full array in memory. This means you can work through large datasets while using less memory.

## How Do Generators Work?

When you use a generator, it processes one piece of data at a time, rather than loading the entire dataset into memory. This is achieved using the yield keyword in PHP.
Using Generators: A Step-by-Step Guide

## 1. Basic Generator Syntax

To create a generator, you define a function with the yield statement. Here's an example:

```
function numberGenerator($start = 1, $end = 10) {
    for ($i = $start; $i <= $end; $i++) {
        yield $i;
    }
}

//And to use it:
foreach (numberGenerator() as $number) {
    echo $number . "<br>";
}
```

## 2. Converting a Regular Function to a Generator

Let’s say you have a function that returns an array:

```
function getNumbers($start = 1, $end = 10) {
    $numbers = [];
    for ($i = $start; $i <= $end; $i++) {
        $numbers[] = $i;
    }
    return $numbers;
}
```

Using this function with a large $end value might cause memory issues. To convert it into a generator:

```
function getNumbersGenerator($start = 1, $end = 10) {
    for ($i = $start; $i <= $end; $i++) {
        yield $i;
    }
}
```

## 3. Yielding Key-Value Pairs

You can also yield key-value pairs:

```
function keyValueGenerator() {
    yield 'a' => 1;
    yield 'b' => 2;
    yield 'c' => 3;
}
```

## Sending Data Back to Generators

Generators are not just for outputting data; they can also receive input during their execution. This is done using the send method.

```
function receiveDataGenerator() {
    while (true) {
        $data = yield;
        if ($data === 'stop') {
            return; // Exit the generator
        }
        echo "Received: $data<br>";
    }
}

$generator = receiveDataGenerator();
$generator->send('Hello');
$generator->send('World');
$generator->send('stop');
```

## Returning Values from Generators

While generators primarily yield values, they can also return a final value:

```
function countdownGenerator($start) {
    for ($i = $start; $i >= 0; $i--) {
        yield $i;
    }
    return 'Blastoff!';
}

$gen = countdownGenerator(5);
foreach ($gen as $value) {
    echo "$value<br>";
}
echo $gen->getReturn(); // Outputs 'Blastoff!'
```

## Practical Use Cases

Generators are especially useful in scenarios like processing large files or handling streams of data. For instance, reading a large file line by line:

```
function readFileLineByLine($filename) {
    $file = fopen($filename, 'r');
    while (!feof($file)) {
        yield fgets($file);
    }
    fclose($file);
}

foreach (readFileLineByLine('large_file.txt') as $line) {
    echo $line . "<br>";
}
```
