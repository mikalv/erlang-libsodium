# libsodium asynchronous port driver

[libsodium](https://libsodium.org) asynchronous port driver for Erlang and Elixir.

*Work In Progress* - use at your own risk.

But if you do, this version uses libsodium 1.0.18 in constrast to the ones this repo is forked from.

* AES128 GCM had to go.
* LIBSODIUM_EXTRA_CONFIGURE_FLAGS environment variable can be passed to tweak libsodium's ./configure.


## Installation

Add `libsodium` to your project's dependencies in `mix.exs`

```elixir
defp deps do
  [
    {:libsodium, github: "mikalv/erlang-libsodium", branch: "improved"},
  ]
end
```

Add `libsodium` to your project's dependencies in your `Makefile` for [`erlang.mk`](https://github.com/ninenines/erlang.mk) or the following to your `rebar.config`

```erlang
{deps, [
  {libsodium, ".*", {git, "git://github.com/potatosalad/erlang-libsodium.git", {branch, "master"}}}
]}.
```

## Usage

```erlang
%% Ed25519 keypair generation
{PublicKey, SecretKey} = libsodium_crypto_sign_ed25519:keypair().
% {<<26,21,68,181,145,231,162,169,205,251,76,23,129,102,225,
%    34,161,174,195,167,92,150,142,192,87,139,99,59,173,193,
%    69,171>>,
%  <<137,111,208,141,44,128,10,206,235,152,68,9,101,26,105,
%    24,213,241,150,41,84,40,38,144,76,32,249,196,240,168,
%    111,42,26,21,68,181,145,231,162,169,205,251,76,23,129,
%    102,225,34,161,174,195,167,92,150,142,192,87,139,99,59,
%    173,193,69,171>>}

%% Ed25519 message signing (combined)
Message = <<"text">>,
SignedMessage = libsodium_crypto_sign_ed25519:crypto_sign_ed25519(Message, SecretKey).
% <<248,19,112,107,154,158,129,116,85,49,41,101,55,159,182,
%   137,175,127,222,65,89,253,126,183,184,135,154,6,193,176,
%   196,251,188,36,78,53,62,236,213,33,80,249,145,237,71,54,
%   88,143,4,251,152,156,138,247,186,220,122,155,92,252,255,
%   215,203,3,116,101,120,116>>

%% Ed25519 message signing (detached)
Signature = libsodium_crypto_sign_ed25519:detached(Message, SecretKey).
% <<248,19,112,107,154,158,129,116,85,49,41,101,55,159,182,
%   137,175,127,222,65,89,253,126,183,184,135,154,6,193,176,
%   196,251,188,36,78,53,62,236,213,33,80,249,145,237,71,54,
%   88,143,4,251,152,156,138,247,186,220,122,155,92,252,255,
%   215,203,3>>

%% Ed25519 message verification (combined)
Message = libsodium_crypto_sign_ed25519:open(SignedMessage, PublicKey).

%% Ed25519 message verification (detached)
0 = libsodium_crypto_sign_ed25519:verify_detached(Signature, Message, PublicKey).
```
