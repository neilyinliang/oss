# Configure server-side encryption {#concept_r55_np5_xgb .concept}

OSS can perform server-side encryption on data uploaded to buckets to ensure data security.

OSS supports server-side encryption for uploaded data. This means that when user data is uploaded, OSS encrypts the data and permanently stores the data. Then, when the data is downloaded by a user, OSS automatically decrypts the data, returns the original data to the user, and declares in the header of the returned HTTP request that the data has been encrypted on the server.

**Note:** For more information about server-side encryption, see [Server-side encryption](../../../../intl.en-US/Developer Guide/Data encryption/Server-side encryption.md#).

You can enable server-side encryption on the OSS console in the following two methods:

-   Method 1: [Enable server-side encryption when creating a bucket](#).
-   Method 2: [Enable server-side encryption in the Basic Settings tab page](#).

## Method 1: Enable server-side encryption when creating a bucket {#section_osb_tr5_xgb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  Click **Create Bucket**.
3.  In the **Create Bucket** dialog box, input the parameters of the bucket.

    Select the following options for **Server Encryption**. For the settings of other parameters, see [Create a bucket](intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#).

    -   **None**: Do not enable server-side encryption.
    -   **AES256**: Encrypt objects using the AES256 algorithm. OSS uses different keys to encrypt each object. Furthermore, the keys are encrypted by a customer master key \(CMK\) that is updated periodically for higher security.
    -   **KMS**: You can use a specified CMK ID or the default CMK managed by KMS to encrypt or decrypt data. For more information about KMS encryption algorithm, see [Principle](../../../../intl.en-US/Developer Guide/Data encryption/Server-side encryption.md#section_c24_wbd_5gb).

        -   alias/acs/oss: Use the default CMK managed by KMS to encrypt different objects, and decrypt the objects when they are downloaded.
        -   CMK ID: Use keys generated by a specified CMK to encrypt different objects, and record the specified CMK ID in the metadata of the encrypted object. Therefore, the objects are automatically decrypted when they are downloaded by users with the decryption permission. Before specifying a CMK ID, you must create a normal key or an [external key](../../../../intl.en-US/User Guide/Import key material.md#) in the same region as the bucket in the [KMS console](https://kms.console.aliyun.com). This function is in the beta testing phase. To join the test, contact [Alibaba Cloud technical support](https://selfservice.console.aliyun.com/ticket/createIndex).
        **Note:** 

        -   Before using the KMS encryption method, you must [activate KMS](https://common-buy.aliyun.com/?spm=a2c4g.11186623.2.12.32745439b1xb3c&commodityCode=kms#/open).
        -   API calling fees are incurred when you use CMKs to encrypt or decrypt data.
4.  Click **OK**.

## Method 2: Enable server-side encryption in the Basic Settings tab page {#section_twt_yx1_ygb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the left-side bucket list, select the bucket that you want to enable server-side encryption for.
3.  Click **Basic Settings**, and then click **Configure** in the **Server Encryption** area.
    -   **None**: Do not enable server-side encryption.
    -   **AES256**: Encrypt objects using the AES256 algorithm. OSS uses different keys to encrypt each object. Furthermore, the keys are encrypted by a customer master key \(CMK\) that is updated periodically for higher security.
    -   **KMS**: You can use a specified CMK ID or the default CMK managed by KMS to encrypt or decrypt data. For more information about KMS encryption algorithm, see [Principle](../../../../intl.en-US/Developer Guide/Data encryption/Server-side encryption.md#section_c24_wbd_5gb).

        -   alias/acs/oss: Use the default CMK managed by KMS to encrypt different objects, and decrypt the objects when they are downloaded.
        -   CMK ID: Use keys generated by a specified CMK to encrypt different objects, and record the specified CMK ID in the metadata of the encrypted object. Therefore, the objects are automatically decrypted when they are downloaded by users with the decryption permission. Before specifying a CMK ID, you must create a normal key or an [external key](../../../../intl.en-US/User Guide/Import key material.md#) in the same region as the bucket in the [KMS console](https://kms.console.aliyun.com). This function is in the beta testing phase. To join the test, contact [Alibaba Cloud technical support](https://selfservice.console.aliyun.com/ticket/createIndex).
        **Note:** 

        -   Before using the KMS encryption method, you must [activate KMS](https://common-buy.aliyun.com/?spm=a2c4g.11186623.2.12.32745439b1xb3c&commodityCode=kms#/open).
        -   API calling fees are incurred when you use CMKs to encrypt or decrypt data.
4.  Click **Save**.

    **Note:** Encryption methods for existing objects in a bucket are not added or modified when you enable or modify the default encryption method for the bucket.


## Permissions {#section_xnc_21b_ygb .section}

To use server-side encryption with a RAM user, you must have the following permissions:

-   To configure the default encryption method for a bucket, you must have:
    -   The management permission of the bucket.
    -   The permission to perform the PutBucketEncryption and GetBucketEncryption operations.
    -   The permission to perform the ListKeys, Listalias, ListAliasesByKeyId, and DescribeKeys operations when you use a specified CMK ID to encrypt data. The RAM policy used to set permissions for specified CMK IDs is as follows:

        ``` {#codeblock_16z_2ut_of2}
        {
          "Version": "1",
          "Statement": [
            {
              "Effect": "Allow",
              "Action": [
                "kms:List*",
                "kms:DescribeKey"    
              ],
              "Resource": [
                "acs:kms:*:1416614965936597:*" //This example allows the user to use all CMKs under the account. To restrict the user to use only one CMK, input the CMK ID.
              ]
            }
          ]
        }
        ```

-   To upload an object to a bucket with the default encryption method configured, you must have:
    -   The permission to upload objects to the bucket.
    -   The permission to perform the ListKeys, Listalias, ListAliasesByKeyId, DescribeKeys, and GenerateDataKey operations when you use a specified CMK ID to encrypt data. Otherwise, the object fails to be uploaded. The RAM policy used to set permissions for specified CMK IDs is as follows:

        ``` {#codeblock_fy5_opn_um8}
        {
          "Version": "1",
          "Statement": [
            {
              "Effect": "Allow",
              "Action": [
                "kms:List*",
                "kms:DescribeKey",
                "kms:GenerateDataKey"
              ],
              "Resource": [
                "acs:kms:*:1416614965936597:*"//This example allows the user to use all CMKs under the account. To restrict the user to use only one CMK, input the CMK ID.
              ]
            }
          ]
        }
        ```

-   To download an object from a bucket with the default encryption method configured, you must have:
    -   The permission to access objects in the bucket.
    -   The permission to perform the Decrypt operation when you use a specified CMK ID to decrypt data. Otherwise, the object fails to be downloaded. The RAM policy used to set permissions for specified CMK IDs is as follows:

        ``` {#codeblock_gni_wfj_1tx}
        {
          "Version": "1",
          "Statement": [
            {
              "Effect": "Allow",
              "Action": [
            "kms:Decrypt"
              ],
              "Resource": [
                "acs:kms:*:1416614965936597:*"//This example indicates that the RAM user has the permission to decrypt data using all CMKs. To decrypt data using a specified CMK, input the CMK ID.
              ]
            }
          ]
        }
        ```


## Reference {#section_o3n_4y1_ygb .section}

For more information, see [Protect data by performing server-side encryption](../../../../intl.en-US/Best Practices/Data security/Protect data by performing server-side encryption.md#).

