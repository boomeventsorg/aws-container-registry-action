# AWS Container registry configuration GitHub action
🍦 GitHub action used for easier AWS Elastic Container Registry configuration and authentication.

This GitHub action uses `aws-actions/configure-aws-credentials@v1` and `aws-actions/amazon-ecr-login@v1` actions and combines them into one for easy use.

## Example usage

```yaml
name: 'My awesome deployment action'

on: push:

jobs:
  build:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2

      # Configuration stage
      - name: Configure AWS Elastic Container Registry
        id: aws-registry
        uses: goforboom/aws-container-registry-action@v1
        with:
          awsAccessKeyId: xyz
          awsSecretAccessKey: xyz
          awsRegion: eu-west-1

      # Build stage
      - name: Build application
        run: docker-compose build

      # Release stage
      - name: Build application image
        env:
          REGISTRY: ${{ steps.aws-registry.outputs.awsRegistry }}
        run: |
          docker tag app:latest $REGISTRY/awesome-application:1
          docker push $REGISTRY/awesome-application:1
```
