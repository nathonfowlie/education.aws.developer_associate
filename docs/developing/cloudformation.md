# CloudFormation

## Overview

- Declarative way to define infrastructure.
- Templates are uploaded to S3.
- Stacks are identified by a name.
- Resources created by the stack are given a tag to identify the stack that created them.
- When a stack is deleted, all artifacts created as part of the stack will be deleted.
-  Use CLI to deploy templates.

## Benefits

### Infrastructure as Code

- No manual creation, good for control.
- All code is version controlled.
- Infra changes can be peer reviewed.

### Cost

- Each resource in the stack is tagged so it's easy to see how much a stack costs.
- Costs can be estimated using the CloudFormation template.
- Can safely automate the deletion of templates outside of business hours to save money.

### Productivity

- Destroy and re-create infra as needed.
- Automated generation of infrastructure diagrams.
- Declarative programming, so no need to figure out the ordering and orchestration).

### Separation of concern

- Create many stacks for many apps/layers (vpc, network, app etc).

### Re-usable

- Leverage existing templates, don't re-invent the wheel.

## Building Blocks

### Template Components

- Resources: mandatory section
- Parameters: Dynamic template inputs
- Mappings: Static inputs
- Outputs: References what was created.
- Conditions: Conditions to control resource creation
- Metadata:

## Template Helpers

- References: Link to sections in the template
- Functions: Transform template data.


## Resources

- AWS components that will be created/configured.
- Resources are declared and can reference each other.
- AWS figures out the order that resources should be created/updated/deleted.
- Over 224 different types of resources (AWS::product-name::data-type).

## Parameters

- Provide inputs to CF templates.
- Parameters have a type, description, constraints, constraint description, min/max length, min/max value, defaults, allow values, allowed patterns and no echo toggle.
- Reference a parameter using ```!Ref``` (example: ```!Ref MyVPC```).
- Pseudo parameters can be used at any time and enabled by default (AWS::AccountId, AWS::Region, AWS::StackName etc).

## Mappings

- Static values defined in the CF template.
- Use ```!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]``` to reference mapping values (example: ```!FindInMap [RegionMap, !Ref "AWS::Region", 32]```).

### Example

    RegionMap:
      us-east-1:
        "32": "ami-6411e20d"
        "64": "ami-7a11e213"
      us-west-1:
        "32": "ami-c9c7978c"
        "64": "ami-31c2f645"

## Outputs

- Optional
- If they're exported, they can be consumed in other stacks.
- Good for separating stacks (refer to VPC id created by the the network stack, in the application stack).
- Can't delete a stack if its outputs are referenced somewhere else.

### Example

    # Stack 1
    Outputs:
      StackSSHSecurityGroup:
        Description: The SSH Security Group for our Company.
        Value: !Ref MyCompanyWideSSHSecurityGroup
        Export:
          Name: SSHSecurityGroup
    
    # Stack 2
    Resources:
      MySecureInstance:
        Type: AWS::EC2::Instance
        Properties:
          SecurityGroups:
            - !ImportValue SSHSecurityGroup
            
## Conditions

- Control the creation of resources/outputs.
- If dev, if test etc.
- And, Equals, If, Not, Or

### Example

    Conditions:
      CreateProdResources: !Equals [ !Ref EnvType, prod ]
    Resources:
      MountPoint:
        Type: "AWS::EC2::VolumeAttachment"
        Condition: CreateProdResources

## Instrinsic Functions

| Function                              | Sample                                               | Notes                                                                                                     |
|---------------------------------------|------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Ref                                   | ```!Ref EC2Instance```                               | If referencing a parameter, returns the param value. If referencing a resources, returns the resource id) |
| GetAtt                                | ```!GetAtt EC2Instance.AvailabilityZone```           | Get the attributes of a resource.                  | 
| FindInMap                             | ```!FindInMap [RegionMap, !Ref "AWS::Region", 32]``` | Get a name value using a key.                      |
| ImportValue                           | ```!ImportValue SSHSecurityGroup```                  | Import values that are exported in other templates. |
| Join                                  |  ```!Join [ ";", [ a, b, c ] ]``` | Join values using a delimiter. |
| Sub                                   | ```!Sub String``` | Substitute values in strings. |
| Conditions (If, Not, Equals, And, Or) | ```!And, !If, !Not, !Or, !Equals``` | Logical operations. |


## Rollbacks

## Stack Creation Failures

- By default, the stack will roll-back and everything is deleted. Refer to the log for error messages.
- Can disable the automatic rollback to troubleshoot the issue.

## Stack Update Failures

- Stack will rollback to the previous known working state.
- Refer to log for error messages.

## ChangeSets

- Shows what will happen if a stack is updated.


## Nested Stacks

- Stacks that are part of other stacks.
- Update/isolate repeated patterns (re-usable security groups etc).
- Considered best practise.
- Different to Cross Stacks, which are for when stacks have different lifecycles.
- Nested stack is only important to the parent stack. It isn't shared.

## StackSets

- Create/update/delete stacks across multiple accounts/regions in a single operation.
- Requires administration account to create them.
- Trusted accounts can create, update or delete stack instances from StackSets.
