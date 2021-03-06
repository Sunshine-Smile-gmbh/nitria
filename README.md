# Nitria PHP7 Code Generator

[![Build Status](https://travis-ci.org/gperler/nitria.svg?branch=master)](https://travis-ci.org/gperler/nitria)
[![License](https://poser.pugx.org/gm314/nitria/license)](https://packagist.org/packages/gm314/siesta)
[![Latest Stable Version](https://poser.pugx.org/gm314/nitria/v/stable)](https://packagist.org/packages/gm314/siesta)
[![Total Downloads](https://poser.pugx.org/gm314/nitria/downloads)](https://packagist.org/packages/gm314/siesta)
[![Latest Unstable Version](https://poser.pugx.org/gm314/nitria/v/unstable)](https://packagist.org/packages/gm314/siesta)

Class generator for PHP7 code. It will take care of indentation, use statements, php doc creation.

# Installation

````json
{
    "require": {
        "gm314/nitria": "*"
    }
}
````


# Usage

Create a class and set extends and implements
````php

$classGenerator = new ClassGenerator("Generated\\MyClass", true);

// add extends statement, a use statement will be added automatically 
$classGenerator->setExtends("BaseClass\\ClassName");

// add implement statement
$classGenerator->addImplements("\\Serializable");

// add a constant
$classGenerator->addConstant("CONSTANT_STRING", '"hello"');
````

# Properties

Properties have a name, a type, a modifier and an optional default value. The example
will generate a `static` `private` property with the name `myName` and the default
value `19.08`.

````php
 $classGenerator->addProperty("myName","float","private", false, '19.08', 'doc bloc comment');
````

This will result in the following code

````php
    /**
     * @var float doc bloc comment
     */
    private $myName = 19.08;
````       

Or you can use the short version for non static properties.
````php
// add property (for classes use statement will be added - unless the class is in the same namespace)
$classGenerator->addPrivateProperty("iAmPrivat", 'MyPackage\MyClass');
$classGenerator->addProtectedProperty("iAmProtected", "array", []);
$classGenerator->addPublicProperty("iAmPublic", "float");
````

And for the static properties.
````php
// add static property
$classGenerator->addPrivateStaticProperty("iAmPrivatStatic", "int");
$classGenerator->addProtectedStaticProperty("iAmProtectedStatic", "bool");
$classGenerator->addPublicStaticProperty("iAmPublicStatic", "array");
````

# Methods


````php
$method = $classGenerator->addPublicMethod("myFunction");
$method->addParameter("string", "parameterName", '"defaultValue!"');
$method->addParameter("\\DateTime", "datetime");

// the method will have a return type string that is not nullable
$method->setReturnType("string", false);
$method->addCodeLine('return $parameterName;');
````

the above code will generate the following method

````php

/**
 * @param string $parameterName
 * @param \DateTime $datetime
 * @return string
 */
public function myFunction(string $parameterName = "defaultValue!", \DateTime $datetime) : string {
    return $parameterName;   
}
````

````php
// method generation
$classGenerator->addPrivateMethod("iAmPrivate");
$classGenerator->addProtectedMethod("iAmProtected");
$classGenerator->addPublicMethod("iAmPublic");

// static method generation
$classGenerator->addPrivateStaticMethod("iAmPrivateStatic");
$classGenerator->addProtectedStaticMethod("iAmProtectedStatic");
$classGenerator->addPublicStaticMethod("iAmPublicStatic");
````


# Method Content generation

## Code

````php
$method = $classGenerator->addPublicMethod("sayIf");
$method->addParameter("int", "intParam");
$method->setReturnType("int", false);

// add a simple line of code
$method->addCodeLine('return $intParam * $intParam;');
````

## If Statement
````php
$method = $classGenerator->addPublicMethod("sayIf");
$method->addParameter("int", "int");
$method->setReturnType("int", false);

// start if statement >> if ($int ===1) {
$method->addIfStart('$int === 1');
$method->addCodeLine('return 1;');

// add if else statement >> } else if ($int === 2) {
$method->addIfElseIf('$int === 2');
$method->addCodeLine('return 2;');

// add else statement >> } else {
$method->addIfElse();
$method->addCodeLine('return 3;');

// close if statement >> }
$method->addIfEnd();
````


## While Statement

````php
$method = $classGenerator->addPublicMethod("sayWhile");
$method->addParameter("int", "int");
$method->setReturnType("string", false);

$method->addCodeLine('$string = "";');

// start while statement >> while($int++ < 10) {
$method->addWhileStart('$int++ < 10');
$method->addCodeLine('$string .= "x";');

// end while statement >> }
$method->addWhileEnd();
$method->addCodeLine('return $string;');
````

## Foreach Statement

````php
$method = $classGenerator->addPublicMethod("sayForeach");
$method->addParameter("array", "list");
$method->setReturnType("string", false);

$method->addCodeLine('$string = "";');

// start foreach >> foreach($list as $item) {
$method->addForeachStart('$list as $item');
$method->addCodeLine('$string .= $item;');

// end foreach >> }
$method->addForeachEnd();
$method->addCodeLine('return $string;');
````


## Switch Statement

````php
$method = $classGenerator->addPublicMethod("saySwitch");
$method->addParameter("string", "value");
$method->setReturnType("string", false);

// start switch statement >> switch($value) {
$method->addSwitch('$value');

// case statement >> case "a":
$method->addSwitchCase('"a"');
$method->addCodeLine('return "a";');

// case break >> break;
$method->addSwitchBreak();

// default >> default:
$method->addSwitchDefault();
$method->addCodeLine('return "c";');
$method->addSwitchBreak();

$method->addSwitchEnd();
````

# Tests / More examples

See under tests/End2End
* GeneratorTest
* CodeTest


# License

MIT
