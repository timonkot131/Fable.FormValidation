# Fable.FormValidation
A Fable React hook library for validating UI inputs and displaying error messages

## Sample Form Validation

![image](https://user-images.githubusercontent.com/1030435/112769901-b4e92280-8ff1-11eb-8b00-7913a44d9b20.png)

## Call the `useValidation()` hook

``` fsharp
open Fable.FormValidation

[<ReactComponent>]
let Page() = 
    let model, setModel = React.useState init
    let rulesFor, validate, resetValidation, errors = useValidation() 

    let save() = 
        if validate() 
        then Toastify.success "Form is valid!"
        else Toastify.error "Please fix validation errors."
        
    let cancel() = 
        resetValidation()
        setModel init


```

## Add validation rules to inputs:

``` fsharp
    input [
        Ref (rulesFor "First Name" [Required; MinLen 2; MaxLen 50])
        Class B.``form-control``
        Value model.FName
        OnChange (fun e -> setModel { model with FName = e.Value })
    ]
```

``` fsharp
      input [
          Ref (rulesFor fieldName [ 
              Required 
              Regex(@"^\S+@\S+$", "Email")
          ])
          Class B.``form-control``
          Value model.Email
          OnChange (fun e -> setModel { model with Email = e.Value })
      ]
```

This example features the Feliz date input with a custom rule:
``` fsharp 
    Html.input [
        prop.ref (rulesFor fieldName [
            Required
            CustomRule (
                match model.BirthDate with
                | Some bd -> if bd <= DateTime.Now then Ok() else (Error "Birth Date cannot be a future date")
                | None -> Ok()
            )
        ])
        prop.className "date"
        prop.type'.date
        if model.BirthDate.IsSome
        then prop.value model.BirthDate.Value
        prop.onChange (fun value ->
            let success, bd = DateTime.TryParse value
            if success then setModel { model with BirthDate = Some bd }
        )
    ]
```

## Add an optional `errorSummary`

``` fsharp
  errorSummary errors
```

## Profit!

![image](https://user-images.githubusercontent.com/1030435/112769923-dea24980-8ff1-11eb-9cf0-3c8040553716.png)
