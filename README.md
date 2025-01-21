Scripts to fetch AWS data and package it as a Batfish snapshot.

## Setup

- Python3 

- `pip install -r requirements.txt`
 
- [Set up AWS credentials](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html) 
  - run `python aws_data_getter.py -t` to see if the credentials are properly setup

## Fetching AWS data

 - `python aws_data_getter.py` 

    This command will take a snapshot of your AWS configurations and put it in `aws-snapshot` folder. To specify a different folder, use the `-o` option. 

    The command reads its config data from `config.json`. You may supply a different file using the `-c` option. You may change the contents of this file to control which regions and VPCs the data is fetched from. Unless you know what you are doing, do not mess with the `skipData` configuration.  

    The command used the default AWS profile by default. To use a different (configured) profile, use the `-p` option.

## Fetching From Multiple Accounts

You can download data from multiple accounts if they are part of an AWS Organization and you have the necessary permissions. Add an `accounts` key to your `config.json` file to configure this. An empty list for `accounts` indicates that the tool should download from all accounts within the organization. Use the `defaultRole` key to specify the role that should be assumed for each account when taking a snapshot.

When fetching from multiple accounts, each account will have its own directory under the root output directory. The name of the account directory can either be the human-friendly name or the account ID using the `useAccountName` option. By default, `useAccountName` is false.

### Example 1: Downloading from All Accounts
The following configuration will download snapshots from all accounts within the organization by assuming the role `Admin`. Accounts will be outputted with human-friendly names.:

```json
{
  "accounts": [],
  "defaultRole": "Admin",
  "useAccountName": true
}
```


### Example 2: Filtering Specific Accounts and Overriding Roles

You can also specify particular accounts and override the default role for some accounts. For example:

```json
{
  "accounts": [
 {
      "id": "1234",
      "role": "Engineer"
 },
 {
      "id": "5678"
 }
 ],
  "defaultRole": "Admin"
}
```

In this configuration:

* Only accounts `1234` and `5678` will be processed.
* The role `Engineer` will be assumed for account `1234`.
* The default role `Admin` will be assumed for account `5678`.
* The account directories will be named after the account Ids `1234` and `5678`.