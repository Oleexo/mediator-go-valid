# Mediator for Go - Validation Pipeline

This project is designed to validate requests during their execution using
the [validator](https://github.com/go-playground/validator) package. By integrating this validation pipeline, you can
ensure input data adheres to defined rules and constraints, improving the reliability and consistency of your
application.

## Getting Started

### Prerequisites

Mediator for Go requires **Go version 1.24** or above. Make sure your environment meets this requirement before
proceeding.

### Installing Mediator for Go - Validation Pipeline

You can install Mediator for Go with [Go's module support](https://go.dev/wiki/Modules#how-to-use-modules). Dependencies
will be handled automatically when you include the import statement in your code:

```go
import "github.com/Oleexo/mediator-go-valid"
```

Alternatively, to add the package manually to your project, run the following command in your terminal:

```sh
go get -u github.com/Oleexo/mediator-go-valid
```

After running the command, the package will be downloaded and added to your `go.mod` file automatically.

## Usage

### Registering the Validation Pipeline

You can register the validation pipeline to the Mediator container. Here's how to integrate it into your application:

```go
package main

import (
	"github.com/Oleexo/mediator-go"
	mediatorvalid "github.com/Oleexo/mediator-go-valid"
)

func main() {
	// Create a new Mediator container and register the validation pipeline
	container := mediator.NewSendContainer(
		mediator.WithPipelineBehavior(mediatorvalid.NewValidationPipeline()),
	)

	// Your application logic goes here
}
```

### Explanation of Components

- **`mediator.NewSendContainer`**: Initializes a container for sending requests with behaviors or middleware pipelines
  attached.
- **`mediator.WithPipelineBehavior`**: Registers custom behaviors (like validation) into the mediator pipeline. In this
  case, it uses `mediatorvalid.NewValidationPipeline()` to set up the validation rules.

### Example: Adding Validation in Actions

Once you have registered the validation pipeline, any requests passed to the Mediator will be validated automatically
based on the structural tags defined in your request types using the well-known `go-playground/validator` package.
Here's an example:

```go
type ExampleRequest struct {
	Name  string `validate:"required"`     // Name must be provided
	Email string `validate:"required,email"` // Email must be valid
}
```

These tags will trigger the validation checks automatically when the request is processed.

### Error Handling

If validation fails, the mediator will return corresponding validation errors. Be sure to handle these errors in your
code to provide appropriate feedback or logging.