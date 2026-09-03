## Ingress & Egress traffic rules
- There are multiple ways we can configure the rules for ingress and egress traffic:
    - Pod Selector: use matchLabels and matchExpressions
    - Namespace Selector: use matchLabels and matchExpressions
    - IP Block: use CIDR blocks
    - Ports and Protocols: define specific ports and protocols
- Different list elements under the from or to conditions behave as an OR condition, while multiple selection options under the same list element behave as an AND condition.
