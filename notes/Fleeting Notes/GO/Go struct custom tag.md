# Go struct custom tag
Created: 2022-12-26 10:49
Tags: 
____

first we should understand what we can do with reflection
[[Go reflect on struct]]

custom validator

```go
package validator

import (
	"errors"
	"fmt"
	"reflect"
	"strings"
)

var (
	ErrNotStruct            = errors.New("not struct")
	ErrInvalidTag           = errors.New("in valid tag")
	ErrInValidField         = errors.New("in valid field")
	ErrValidatorTypeNotMath = errors.New("type doesn't match")
)

type validator interface {
	validate(reflect.Value) error
}

type ErrValidation struct {
	fieldNameToError map[string]error
}

func (ev *ErrValidation) Error() string {
	var res strings.Builder
	for filedName, ErrorStr := range ev.fieldNameToError {
		res.WriteString(
			fmt.Sprintf("filed=%s, err=%v\n", filedName, ErrorStr),
		)
	}

	return res.String()
}

func Validate(obj any) error {
	if reflect.TypeOf(obj).Kind() != reflect.Struct {
		return ErrNotStruct
	}

	objectValue := reflect.ValueOf(obj)
	objectType := reflect.TypeOf(obj)

	filedToError := make(map[string]error)

	for i := 0; i < objectValue.NumField(); i++ {
		field := objectType.Field(i)
		name := objectType.Field(i).Name

		tag := field.Tag.Get("validation")

		var v validator
		switch field.Type.Kind() {
		case reflect.Int:
			var max, min int64
			if n, err := fmt.Sscanf(tag, "min=%d,max=%d", &min, &max); err != nil || n != 2 {
				return fmt.Errorf("for %s ,tag %s is invalid: %w", name, tag, ErrInvalidTag)
			}
			v = &intValidator{min: min, max: max}
		case reflect.String:
			var maxLen int
			if n, err := fmt.Sscanf(tag, "maxLen=%d", &maxLen); err != nil || n != 1 {
				return fmt.Errorf("for %s,tag %s is invalid: %w", name, tag, ErrInvalidTag)
			}
			v = &stringValidator{maxLen: maxLen}
		}

		if v != nil {
			if err := v.validate(objectValue.Field(i)); err != nil {
				filedToError[name] = err
			}
		}

	}

	if len(filedToError) > 0 {
		return &ErrValidation{fieldNameToError: filedToError}
	}

	return nil
}

type intValidator struct {
	min int64
	max int64
}

func (iv *intValidator) validate(field reflect.Value) error {
	if field.Kind() != reflect.Int {
		return fmt.Errorf("value is not int: %w", ErrValidatorTypeNotMath)
	}

	intValue := field.Int()

	if iv.min > intValue || intValue > iv.max {
		return fmt.Errorf("value %d must be in [%d, %d]", intValue, iv.min, iv.max)
	}

	return nil
}

type stringValidator struct {
	maxLen int
}

func (sv *stringValidator) validate(field reflect.Value) error {
	if field.Kind() != reflect.String {
		return fmt.Errorf("value is not int: %w", ErrValidatorTypeNotMath)
	}

	strValue := field.String()
	if len(strValue) > sv.maxLen {
		return fmt.Errorf("string len is %d, but must be less than %d: %w", len(strValue), sv.maxLen, ErrInValidField)
	}

	return nil
}

```



```go
package main

import (
	"fmt"

	"github.com/ahmadrezam97/goclass/e/validator"
)

type Person struct {
	Name string `validation:"maxLen=2"`
	Age  int    `validation:"min=0,max=100"`
}

func main() {

	p := Person{Name: "ahmadreza", Age: 102}
	if err := validator.Validate(p); err != nil {
		validationErr, ok := err.(*validator.ErrValidation)
		if ok {
			fmt.Println(validationErr.Error())
		} else {
			fmt.Println("simple err", err)
		}

	}

}
```
_____
##### References
1.

