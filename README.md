# General Information

YiiConditionalValidator (YCV) validates some attributes depending on certains conditions (rules). You can use any core validator as you usually would do or any other class based or inline validator. You can even use the own YiiConditionalValidator inside itself to perform more complex conditions;

Basically, YCV executes the rules set in the param `if` and if there are no errors executes the rules set in the param `then`.

## Syntax

```php
array('attrZ, attrG', 'path.to.YiiConditionalValidator',
    'if' => array(
        //rule1: array('attrX, attrY', 'required', ...)
        //ruleN: ...
    )
    'then' => array(
        //rule1: array('attrZ, attrG', 'required', ...)
        //ruleN: ...
    )
)
```

- `attribute`: The name of the attributes from `then` attribute (see lower);
- `path.to.YiiConditionalValidator`: In the most of cases will be `ext.YiiConditionalValidator`;
- `if`: (bidimensional array) The conditional rules to be validated. *Only* if they are all valid (i.e., have no errors) then the rules in `then` will be validated;
- `then`: (bidimensional array) The rules that will be validated *only* if there are no errors in rules of `if` param;

> Note:
Errors in the rules set in the param `if` are discarded after checking. Only errors in the rules set in param `then` are really kept.


## Examples

`If` *customer_type* is "active" `then` *birthdate* and *city* are `required`:
```php
public function rules()
{
    return array(
        array('birthdate, city', 'ext.YiiConditionalValidator',
            'if' => array(
                array('customer_type', 'compare', 'compareValue'=>"active"),
            ),
            'then' => array(
                array('birthdate, city', 'required'),
            ),
        ),
    );
}
```

`If` *customer_type* is "inactive" `then` *birthdate* and *city* are `required` **and** *city* must be "sao_paulo", "sumare" or "jacarezinho":
```php
public function rules()
{
    return array(
        array('birthdate, city', 'ext.YiiConditionalValidator',
            'if' => array(
                array('customer_type', 'compare', 'compareValue'=>"active"),
            ),
            'then' => array(
                array('birthdate, city', 'required'),
                array('city', 'in', 'range' => array("sao_paulo", "sumare", "jacarezinho")),
            ),
        ),
    );
}
```

`If` *information* starts with 'http://' **and** has at least 24 chars length `then` the own *information* must be a valid url:
```php
public function rules()
{
    return array(
        array('information', 'ext.YiiConditionalValidator',
            'if' => array(
                array('information', 'match', 'pattern'=>'/^http:\/\//'),
                array('information', 'length', 'min'=>24, 'allowEmpty'=>false),
            ),
            'then' => array(
                array('information', 'url'),
            ),
        ),
    );
}
```

## Installation
1. Put YiiConditionalValidator.php in your application.extensions folder;

## Requirements
- Tested in Yii 1.1.14;

## Help and reference for original YCV
- [Forum](http://www.yiiframework.com/forum/index.php?/topic/27930-extension-yii-conditional-validator/)
- [Fast Validators Reference](http://www.yiiframework.com/wiki/56/reference-model-rules-validation/)

## Change Log
[Version 1.2.0]
- Improved clientside validation

[Version 1.1.0]
- Remove dot notation support.
- Add client validation

[Version 1.0.0]
- Usage made yet more easier, simplyfied and objective;
- New `if`/`then` operators replace `validations`/`dependentValidations` making the use more natural;
- Code completely refactored and (almost) commented;
- Allows to use multiple attributes and/or validator combinations in the same set of YCV rule;

[Version 0.2.0]
- Usage made easier, more simplyfied and more objective;
- New 'dot.notation' usage on attributes name (will be improoved on next versions);
- Some bug fixes;

## ToDo
- Implement operator ('and', 'or') for multiple rules in `if` and in `then`;
