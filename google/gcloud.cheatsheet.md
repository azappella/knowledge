# gcloud cheatsheet

## get org policies for user 

```
gcloud organizations get-iam-policy "<organization_id>" \
        --filter="bindings.members:<user@example.com>" \
        --flatten="bindings[].members" \
        --format="value(bindings.role)"
```

## get billing accounts policies for user

```
# format for the billing account id: XXXXXX-XXXXXX-XXXXXX
gcloud beta billing accounts get-iam-policy "<billing_account_id>" \
        --filter="bindings.members:<user@example.com>" \
        --flatten="bindings[].members" \
        --format="value(bindings.role)"


```

## add policy binding for user

```
gcloud RESOURCE_TYPE add-iam-policy-binding RESOURCE_ID \
    --member=PRINCIPAL --role=ROLE_ID \
    --condition=CONDITION

RESOURCE_TYPE: The resource type that you want to manage access to. Use projects, resource-manager folders, or organizations.

RESOURCE_ID: Your Google Cloud project, folder, or organization ID. Project IDs are alphanumeric, like my-project. Folder and organization IDs are numeric, like 123456789012.

PRINCIPAL: An identifier for the principal, or member, which usually has the following form: PRINCIPAL_TYPE:ID. For example, user:my-user@example.com. For a full list of the values that PRINCIPAL can have, see the Policy Binding reference.
```