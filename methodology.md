# Bug Bounty Methodology

## Attack Vectors

### 1. EngineX 403 Bypass
Double URL-encoded path traversal bypass

### 2. Hardcoded Credentials
Check docs and JS files for credentials

### 3. Numeric IDOR
Check numeric user IDs for access control issues

### 4. Blind XSS
Check all user input points including internal panels

### 5. SQL Injection via Protobuf
Binary format: param name + null byte + value length + value

### 6. GraphQL Security Testing
- Introspection - Get full schema with __schema query
- Batch attacks - Multiple queries in one request
- Deep recursion - DoS via nested queries
- Field suggestions - Schema leakage (leaks valid fields)
- Alias flooding - Rate limit bypass
- @include/@skip - Directive manipulation
- Query complexity - DoS via nested depth

Tools: GraphQL Voyager, InQL, graphql-path-enum

### 7. S3 Bucket Enumeration

Classic:
```bash
aws s3 ls s3://target-bucket-name --no-sign-request
```

Permutation script:
```bash
for word in $(cat buckets.txt); do
  aws s3api list-objects-v2 --bucket $word-target --no-sign-request 2>/dev/null && echo "HIT: $word-target"
done
```

Common bucket names: target-static, target-assets, target-uploads, target-logs, target-backup-2026

Misconfig gold: Public ACL + ListBucket + GetObject = free data exfil