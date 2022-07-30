# nns_test_local_m1

terminal 1:
```sh
~/.cache/dfinity/versions/0.9.3/ic-starter \
    --replica-path ~/.cache/dfinity/versions/0.9.3/replica \
    --subnet-features ecdsa_signatures \
    --dkg-interval-length=20 \
    --subnet-type system \
    --create-funds-whitelist '*'
```

terminal 2:
```sh
~/.cache/dfinity/versions/0.9.3/icx-proxy \
    --fetch-root-key \
    --address 127.0.0.1:8000 \
    --replica http://localhost:8080
```

terminal 3:
```sh
cd nns_test_local_m1/

./ic-nns-init --url http://localhost:8080/ --wasm-dir=./wasm --initialize-ledger-with-test-accounts  $(dfx ledger account-id)
```

after long time, the canisters in nns will deploy in local, then we can test them. 

## others
the ic-nns-init is build from https://github.com/dfinity/ic/tree/403767ba04ac9dcf9ac864348cd0cefde4cd4b54, and change [this default](https://github.com/dfinity/ic/blob/403767ba04/rs/canister_client/src/http_client.rs#L41) value to `false`, and fix the [LMDB error](https://forum.dfinity.org/t/lmdb-sys-lmdb-libraries-liblmdb-lmdb-h-does-not-exist/12044/4). And the env is mac m1, so the binary file `ic-nns-init` can only run in mac m1.

the most wasm is built from https://github.com/dfinity/ic/tree/403767ba04ac9dcf9ac864348cd0cefde4cd4b54, some is download from:

```sh
export IC_VERSION=a7058d009494bea7e1d898a3dd7b525922979039
curl -o ledger-canister_notify-method.wasm.gz https://download.dfinity.systems/ic/$\{IC_VERSION\}/canisters/ledger-canister_notify-method.wasm.gz
```