
# JWT verifier for Google tokens

![](https://api.travis-ci.org/amezcua/jwt-google-tokens.svg?branch=master)

Elixir library that verifies Google generated JWT tokens (such as those returned by Firebase authentication) and returns the claims data.

The intended use case is to validate signed tokens retrieved by a mobile app using [Firebase Authentication](https://firebase.google.com/docs/auth/), where the app talks directly with the Google Authentication service and retrieves an authentication token (a Json Web Token) that can be later sent to a server for verification or by web apps that use the Firebase [JavaScript API](https://firebase.google.com/docs/auth/web/google-signin).  

JWT tokens are also returned by other Google authentication services and this library could be used to verify them too.

## Usage

```elixir
    iex > {:ok, {claims}} = Jwt.verify token
```

## Installation

The package can be installed as follows (will try to make it available in Hex in a future version):

  1. Add `jwt` to your list of dependencies in `mix.exs`:

    ```elixir
    def deps do
      [{:jwt, git: "https://github.com/amezcua/jwt-google-tokens.git", branch: "master"}]
    end
    ```

  2. Ensure `jwt` is started before your application:

    ```elixir
    def application do
      [applications: [:jwt]]
    end
    ```

## Plug

A plug 

```elixir
Jwt.Plug
```

is included with the library to allow for integration in web frameworks. The plug looks at the authorization HTTP header to see if it includes a value with the format

```elixir
Authorization: Bearer [JWT]
```

where *[JWT]* is a JWT token. If it is there the library will attempt to validate it and attach the claims to the *Plug.Conn* object. The claims can then be accessed with the *:jwtclaims* atom:

```elixir
claims = conn.assigns[:jwtclaims]
name = claims["name"]
```

If the token is invalid, the plug with directly return a 401 response to the client.

The tokens expiration timestamp are also checked to verify that they have not expired. Expired tokens (within a 5 minute time difference) are rejected.

## Limitations

* At this point the library does not validate any extra claims besides the signature.

## License

[Apache v2.0](https://opensource.org/licenses/Apache-2.0)