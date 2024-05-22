# go_zit_backend

We’ll start by creating a **[GitHub repository](https://github.com/brentgroves/go_zit_backend)** and cloning it to your machine.

Use the repository name to initialize the project:

```bash
pushd .

mkdir ~/src/go_zit_backend/src
cd ~/src/go_zit_backend/src
export GOFLAGS=-mod=vendor
export GO111MODULE=on

go mod init github.com/brentgroves/go_zit_backend.git 

```

GOFLAGS
this is nothing but an environment variable(if you don't understand what an environment variable is, think of it like a value that can be accessed by any process in your current environment. These values are maintained by the OS).
So this GOFLAGS variable has a space separated list of flags that will automatically be passed to the appropriate go commands.
The flag we are setting here is mod, this flag is applicable to the go build command and may not be applicable to other go commands.

what does setting -mod=mod flag, actually do during go build?
The -mod flag controls whether go.mod may be automatically updated and whether the vendor directory is used.
-mod=mod tells the go command to ignore the vendor directory and to automatically update go.mod, for example, when an imported package is not provided by any known module.

**[Refer](https://go.dev/ref/mod#build-commands)**

## Vendoring

When using modules, the go command typically satisfies dependencies by downloading modules from their sources into the module cache, then loading packages from those downloaded copies. Vendoring may be used to allow interoperation with older versions of Go, or to ensure that all files used for a build are stored in a single file tree.

The go mod vendor command constructs a directory named vendor in the main module’s root directory containing copies of all packages needed to build and test packages in the main module. Packages that are only imported by tests of packages outside the main module are not included. As with go mod tidy and other module commands, build constraints except for ignore are not considered when constructing the vendor directory.

## GO111MODULE

GO111MODULE is a setting in Go programming language that controls the behavior of Go modules, which is a feature added in Go version 1.11 to help manage dependencies in Go projects.

Before the introduction of Go modules, Go relied on the GOPATH environment variable to manage dependencies. In this model, all Go packages and their dependencies were stored in a single directory called GOPATH. This approach was simple, but it led to several problems, including version conflicts, the need to manually manage dependencies, and difficulties in sharing code.

## Add Go **[SDK](https://github.com/zitadel/zitadel-go)** to your project

You need to add the SDK into Go Modules by:

```bash
pushd .
cd ~/src/go_zit_backend/src
go get -u github.com/zitadel/zitadel-go/v3
go mod tidy
# go mod download
go mod vendor
go mod verify
all modules verified

```

Create example API
Create a new go file, **[main.go](./src/main.go)** with the content below. This will create an API with three endpoints:

/api/healthz: can be called by anyone and always returns OK
/api/tasks: requires authorization and returns the available tasks
/api/add-task: requires authorization with granted admin role and adds the task to the list
If authorization is required, the token must not be expired and the API has to be part of the audience (either client_id or project_id).

For tests we will use a Personal Access Token.

You will need to provide some values for the program to run:

domain: Your ZITADEL instance domain, e.g. <https://my-domain.zitadel.cloud>
<!-- https://github.com/zitadel/zitadel/discussions/7310 
I tried to run my application with the following command:
go run main.go --domain auth.ourdomain.com --key XKv2Lqd7YAq13NUZVUWZEWZeruqyzViM --clientID 251705954687582568@our_org_name --redirectURI http://localhost:8089/auth/callback
-->
key: The path to the downloaded key.json
port: The port on which the API will be accessible, default it 8089

Test API
After you have configured everything correctly, you can simply start the example by:

```bash
# go run main.go --domain <your domain> --key <path>
go run main.go --domain <your domain> --key <path>
```

## NEXT

Be sure to have a valid key JSON and that its service account is either ORG_OWNER or at least ORG_OWNER_VIEWER before you continue with this guide.
<https://zitadel.com/docs/guides/integrate/zitadel-apis/example-zitadel-api-with-go>
<https://zitadel.com/docs/guides/integrate/zitadel-apis/access-zitadel-apis>
<https://zitadel.com/docs/guides/integrate/service-users/private-key-jwt#2-generate-a-private-key-file>

Store key.json outside of repo
