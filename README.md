# database-restore-action
GitHub Actions for checking if database backup can be restored and restored database worked properly

# Table of Contents
- [Table of Contents](#table-of-contents)
- [Getting Started](#getting-started)
    - [On GitHub Actions](#on-gitHub-actions)
    - [On Terminal](#on-terminal)
- [License](#license)
- [Contributors](#Contributors)

## Getting Started
### On GitHub Actions
Create a configuration file as follows.  


```yaml:database.yml
database:               # DB information of restore destination
  type: mysql           # sql driver
  name: test-database   # database name
  user: root            # database user
  password: root    # database passward
  host: 127.0.0.1       # database host
  port: 3307            # database port
check:
- query: "select * from m_campaign" # test query
  operator: exists                  # operator
backup:
    local: path/to/backupFile # Backup file path
```

And set up a workflow file as follows and run restore test on GitHub Actions.

```yaml:./.github/workflows/test.yaml
name: backup restore test
on:
  schedule:
    - cron:  '0 0 12 * *'
jobs:
  test:
    name: Backup test
    runs-on: normal
    services:
      db:
        image: mysql:5.7.33
        ports:
          - 3307:3306
        env:
          MYSQL_ROOT_PASSWORD: root
        options: --health-cmd "mysqladmin ping -h localhost" --health-interval 20s --health-timeout 10s --health-retries 10
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: setup mysql-client 
        run: apt update && apt install -y mysql-client
      - name: backup test
        uses: pepactions/database-restore-action@main
        with:
          config-path: .database/test.yaml
```

### On Terminal

## License

## Contributors
