When looking at the Nutanix packer plugin, I came across the Multistep package from Hashicorp, and really like the elegant code. Here is a basic runner;

```go
package main

import (
	"context"
	"fmt"
	"github.com/hashicorp/packer-plugin-sdk/multistep"
)

type stepAdd struct{}

func (s *stepAdd) Run(ctx context.Context, state multistep.StateBag) multistep.StepAction {
	// Read our value and assert that it is they type we want
	value := state.Get("value").(int)
	fmt.Printf("Value is %d\n", value)

	// Store some state back
	state.Put("value", value+1)
	return multistep.ActionContinue
}

func (s *stepAdd) Cleanup(multistep.StateBag) {
	// This is called after all the steps have run or if the runner is
	// cancelled so that cleanup can be performed.
}

func main() {
	// Our "bag of state" that we read the value from
	state := new(multistep.BasicStateBag)
	state.Put("value", 0)

	steps := []multistep.Step{
		&stepAdd{},
		&stepAdd{},
		&stepAdd{},
	}

	runner := &multistep.BasicRunner{Steps: steps}

	// Executes the steps
	runner.Run(context.Background(), state)
}

```

Especially when trying to split up messy tasks like SSH output, parsing, etc, it could be a really useful tool.