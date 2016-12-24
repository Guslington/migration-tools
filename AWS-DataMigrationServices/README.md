# Data Migration Services
## Rake Tasks to setup and destory AWS Data Migration Services

```bash
$ rake -T
rake dms:create_endpoints  # Create endpoints
rake dms:create_instance   # Create replication instance
rake dms:create_tasks      # Create replication tasks
rake dms:delete_endpoints  # Delete endpoints
rake dms:delete_instance   # Delete replication instance
rake dms:delete_tasks      # Delete replication tasks
rake dms:start_tasks       # Start replication tasks
rake dms:stop_tasks        # Stop replication tasks
rake dms:test_endpoints    # test connections of endpoints
rake dms:update_endpoints  # Modify endpoints
rake dms:update_instance   # Modify replication instance
rake dms:update_tasks      # Modify replication task
```

## Configure with default_params.yml

```bash
cp default_params.example.yml default_params.yml
```

### Region
```yml
region: us-west-2
```

### AWS Credentails, IAM Role or User profile (IAM Role takes precedent if both are configured)
```yml
aws_profile: aws-account
aws_role_arn: arn:iam:::111111111111/role
```

### Parameters for DMS Replication Instance
```yml
dms_instance:
  replication_instance_identifier: example-dms
  publicly_accessible: false
  allocated_storage: 50
  replication_instance_class: dms.t2.medium
  replication_subnet_group_identifier: subnet-id
  vpc_security_group_ids:
    - sg-id
```

### Source and Target endpoint parameters
```yml
source:
  host: source.example.com
  engine: SQLSERVER
  port: 1433
  username: user
  password: pass
  databases:
    source01.example.com:
      - database01
      - database02

target:
  engine: SQLSERVER
  port: 1433
  username: user
  password: pass
  databases:
    target01.example.com:
      - database01
    target02.example.com:
      - database02
```

### Setup table mappings and migration type for DMS Replication tasks
```yml
tasks:
  migration_type:
    default: full-load
  table_mappings:
    default:
      TableMappings:
        -
          Type: Include
          SourceSchema: company
          SourceTable: "emp%"
        -
          Type: Include
          SourceSchema: employees
          SourceTable: "%"
        -
          Type: Exclude
          SourceSchema: source101
          SourceTable: "dep%"
```
