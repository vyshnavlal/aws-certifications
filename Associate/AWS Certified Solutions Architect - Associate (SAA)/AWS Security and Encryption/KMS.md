
# AWS KMS (Key Management Service)
- Anytime you hear "encryption" for an AWS service, it's most likely KMS
- AWS manages encryption keys for us
- Fully integrated with IAM for authorization
- Easy way to control access to your data
- Able to audit KMS key usage using CloudTrail
- Seamlessly integrated into most AWS services (RDS, EBS, S3...)
- **Never ever store your secret in plain text, especially in your code!**
  - KMS key encryption is also available thrrough API calls (SDK/CLI)
  - Encrypted secrets can be stored on the code / environment variables

## KMS Keys Types
- KMS Keys is the new name of KMS Customer Master Key
- Symmetric (AES-256 keys)
  - Single encryption key that is used to encrypt and decrypt
  - AWS services that are integrated with KMS use Symmetric CMKs
  - You never get access to KMS Key unencrypted (must call KMS API to use)
- Asymmetric (RSA && ECC key pairs)
  - Public (Encrypt) and Private (Decrypt) key pairs
  - Used for Encrypt/Decrypt, or Sign/Verify operations
  - The public key is downloadable but you can't access the private key unencrypted
  - Use case: encryption outside of AWS by users who can't call the KMS API
 
 - Three types of KMS Keys:
   - AWS Managed key: free (aws/service-name, example: aws/rds or aws/ebs)
   - Customer Managed Keys (CMK) created in KMS: $1/month
   - Customer Managed Keys imported (must be 256-bit symmetric key): $1/month
- In addition pay for API calls to KMS ($0.03/10000 calls)
- Automatic key rotation
  - AWS managed KMS key: automatic every 1 year
  - Customer managed KMS key: (must be enabled) automatic every 1 year
  - Imported KMS key: only manual rotation is possible using alias

- Copying Snapshots across region

![image](https://user-images.githubusercontent.com/65948438/202118691-c15c4811-34a7-4a6a-b04a-d6535fa3c7f2.png)

## KMS Key Policies
- Control access to KMS keys, "similar" to S3 bucket policies
- Difference: you can't control access without them
- Default KMS key policy:
  - Created if you don't provide a specific KMS key policy
  - Complete access to the key to the root user = entire AWs account
- Custom KMS Key Policy:
  - Define users, roles that can access the KMS key
  - Define who can administer the key
  - Useful for cross-account access of your KMS key

## Copying snapshots across accounts
1. Create a snapshot, encrypted with your own KMS key (Customer Managed Key)
2. Attach a KMS key policy to authorize cross-account access

![image](https://user-images.githubusercontent.com/65948438/202123164-804eb94a-eb4f-44c3-a915-fdda2a150d40.png)

3. Share the encrypted snapshot
4. (in target) Create a copy of the snapshot, encrypt it with a CMK in your account
5. Create a volume from the snapshot

