1. Start Docker Desktop

2. Run
```sh
CI=true make setup && make rg35xx rg35xxplus && make PLATFORMS="rg35xx rg35xxplus" special package
```