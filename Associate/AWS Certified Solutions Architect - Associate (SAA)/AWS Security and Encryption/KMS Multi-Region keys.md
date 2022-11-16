# KMS Multi-Region Keys
- Identical KMS keys in different AWS regions that can be used interchangeably
- Multi-region keys have the same key ID, key material, automatic rotation...
- Encrypt in one region and decrypt in other regions
- No need to re-encrypt or making cross-region API calls
- KMS multi-region are not global (Primary + Replicas)
- Each multi-region is key is managed independently
- Use cases: global client-side encryption, encryption on Global DynamoDB, global Aurora

![image](https://user-images.githubusercontent.com/65948438/202155018-383f5abb-1d1a-4ac1-b687-1bef5db3902e.png)

