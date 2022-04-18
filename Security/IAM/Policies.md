## Policies

- SCPs của organization 
- Resource policies
- IAM identity boundaries 
- Session Policies
- Identity Policies

### Same Account (cùng tài khoản)
1. Explicit Deny được check trước, nếu có sẽ bị deny ngay lập tức
2. Nếu không, tiếp tục check SCP, nếu không có mà không cho phép (Allow) thì sẽ bị deny (implicit). 
3. Nếu SCP cho phép, sẽ tiếp tục check tới resource policies, nếu resource policies cho phép thì được truy cập và process ngưng tại đây
4. Nếu SCP không cho phép, check tới Permissions Boundaries, nếu có boundary và không cho phép thì là implicit deny 
5. Nếu Boundary cho phép thì sẽ tiếp tục check tới Session Policies (IAM Role) và được cho phép sẽ tiếp tục tới Identity Policies



### Different Account (khác tài khoản)

Cần phải có sự cho phép từ hai account


### Policies Evaluation Logic
1. Explicit Deny
2. Organization SCPs
3. Resource Policies
4. IAM Identity Boundaries
5. Session Policies
6. Identity Policies
