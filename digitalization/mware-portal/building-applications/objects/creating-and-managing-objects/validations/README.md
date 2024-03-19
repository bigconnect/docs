# Validations

Object validations set rules for determining valid field inputs. You can define validations with [Groovy scripts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-groovy-validations), [Liferay expressions](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-expression-builder-validations), or a specified [Composite Key validation](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/validations/adding-field-validations#using-composite-key-validations). When triggered, the validation checks for valid field inputs according to defined conditions and displays an error message for invalid inputs.

![Add field validations to object definitions.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/creating-and-managing-objects/validations/images/01.png)

If out-of-the-box validations don’t meet your needs, you can create custom validations using the `objectValidationRule` client extension. See [Microservice Client Extensions](https://learn.liferay.com/web/guest/w/dxp/building-applications/client-extensions/microservice-client-extensions) for more information.
