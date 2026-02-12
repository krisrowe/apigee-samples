# Maven + Apigeelint Integration Sample

This sample demonstrates how to integrate `apigeelint` and the Apigee Deployment Maven Plugin into a standard Maven build lifecycle.

## Key Benefits
- **No Global Dependencies:** Uses `frontend-maven-plugin` to download Node.js and `apigeelint` locally.
- **Systemic Enforcement:** Linting runs automatically during `mvn test`.
- **Standard Project Structure:** Keeps the repository as a Maven project.
- **Integrated Deployment:** Deploy your proxy directly via Maven using standard Google Cloud authentication.

## Project Structure
- `src/main/apigee/apiproxy`: Contains the Apigee API Proxy bundle (Weather API sample).
- `pom.xml`: Configures the build and deployment lifecycle.

## Prerequisites
- **Apache Maven:** Required to run the build. Install via Homebrew: `brew install maven`
- **Google Cloud SDK (gcloud):** Required for the deployment workflow to generate an access token.
- **Node.js:** *Not required* globally. The build will download and manage its own local Node.js environment.

## Usage

### 1. Run Linting (Local only)
No credentials required.
```bash
mvn clean test
```

### 2. Run Linting and Deployment
Requires `gcloud` to be authenticated.
```bash
mvn clean install -Pdeploy \
  -Dapigee.org=your-gcp-project-id \
  -Dapigee.env=your-env-name \
  -Dapigee.bearer=$(gcloud auth print-access-token)
```

## Configuration for Testing
**No credentials are required** for the linting process.

For deployment, the `apigee-edge-maven-plugin` is used in the `deploy` profile. It is configured for Apigee X by default (`apigee.hosturl=https://apigee.googleapis.com`).

**Note:** Never commit project IDs, service account keys, or environment names to this repository.

## Configuration Details
1.  **frontend-maven-plugin**: Installs a local Node.js environment.
2.  **exec-maven-plugin**: Invokes the `apigeelint` CLI.
3.  **apigee-edge-maven-plugin**: (In `deploy` profile) Packages and deploys the proxy.
