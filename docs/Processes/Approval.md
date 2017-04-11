Appclusive provides a freely configurable 16 level approval process (which means, you can implement a 32-eye approval with that - enough for most use cases we can think of). 

An `Approval` is a mechanism that can be defined to control certain interactions of users such as *Order Processes*. Approvals will then suspend the respective process or action and wait until the `Approver` will `Approve` or `Decline` the `Approval`. Only when an `Approval` is `Approved` the corresponding process or action will be resumed and execution can continue. 

# ApprovalTypes

Approvals can be of one of the following types:

 1. AutoApprove
 2. AutoDecline
 
## AutoApprove

An `Approval` of this type will set its `Status` to `Approved` when no action has been taken by the `Approver` and either `AbsoluteExpiration` or `RelativeExpiration` has expired. In that case the `Approval` will continue as if the `Approver` had `Approved` the `Approval`.

## AutoDecline

An `Approval` of this type is the opposite of the former and will set its `Status` to `Declined` when no action has been taken by the `Approver` and either `AbsoluteExpiration` or `RelativeExpiration` has expired. In that case the `Approval` will terminate as if the `Approver` had `Declined` the `Approval`.

# Expiration

An `Approval` can hold either of the following expiration values:

 1. AbsoluteExpiration
 2. RelativeExpiration

## AbsoluteExpiration

With this expiration option, the `Approval` will expire at a specified `DateTimeOffset` expressed in `Ticks` (where each `Tick` is a `100ns` interval). An example could be the end of a year, such as `2018-01-01`, wich would be `636503616000000000` ticks.
 
## RelativeExpiration

With this expiration option, the `Approval` will expire after a specified `TimeRange` expressed in `Ticks` (where each `Tick` is a `100ns` interval) which is calculated from the time the `Approval` was created. A typical example for this might be an approval that should be cancelled (i.e. `Declined`) after 4 weeks without being `Approved`, which would be `24192000000000` ticks.

Hint: an easy ways to calculate `Ticks` is to use PowerShell and `[timespan]::FromDays(3)` or one of the other static methods:

	Name             Definition
	----             ----------
	FromDays         static timespan FromDays(double value)
	FromHours        static timespan FromHours(double value)
	FromMilliseconds static timespan FromMilliseconds(double value)
	FromMinutes      static timespan FromMinutes(double value)
	FromSeconds      static timespan FromSeconds(double value)

# Configuration

Approvals can be configured via *global attributes* which can be defined on the following entities:

 1. Tenant
 2. Organisational Unit (OU)
 3. Catalogue
 4. CatalogueItem
 5. Blueprint / Model

