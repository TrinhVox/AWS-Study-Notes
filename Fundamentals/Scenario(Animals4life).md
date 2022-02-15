# Animal4life

## Scenario - requirements
- IoTs, large dataset
- Global offices
- Field workers around the globe

## Infrastructure
### Head Office - Core Work
- On-premise network 192.168.10.0/24
- AWS Pilot 10.0.0.0/16
- Azure Pilot 172.31.0.0/16

### None Australian Major Offices 
- London
- New York
- Seattle
- All consume service from Head Office => performance is sub-optimal
- Meaning platform needs to be available all the time


## Problems
- Legacy on-premises hardware failing
- Previous AWS/Azure Pilots were not best practices
- Performance is sub-optimal because consume service from Brisbane
- Lack of scalability for offices and field workers across the globe
- Staff are not skilled with cloud practices
- Cost concerns for expansion

## Ideal Outcomes
- Better performances for field workers
- Deploy into new regions when required
- Low cost 
- Scalable base infrastructure
- Agility => Quickly react to different events around the world
- Automation - low staffing costs 