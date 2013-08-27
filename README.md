# Bosh release for a SSL Proxy

One of the fastest ways to get a SSL proxy in front of your CloudFoundry router running on any infrastructure is too deploy this bosh release.

## Usage

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_URL
bosh login
git clone git@github.com:cloudfoundry-community/sslproxy-boshrelease.git
cd sslproxy-boshrelease
bosh upload release releases/sslproxy-1.yml
```

Now update the `examples/openstack*.yml` woth your settings (look up for #CHANGE).

Finally, target and deploy. For deployment to a bosh running on openstack:

```
bosh deployment examples/openstack.yml
bosh deploy
```

### Self-signed certificates by default

By default you do not need to provide a signed SSL certificate. This is very useful for dev/test deployments.

It will mean that Chrome users, for example, will see the red-screen-of-fear. So, its not ideal for production and your lovely end users.


## Development


## Create new final release

To create a new final release you need to get read/write API credentials to the [@cloudfoundry-community](https://github.com/cloudfoundry-community) s3 account.

Please email [Dr Nic Williams](mailto:&#x64;&#x72;&#x6E;&#x69;&#x63;&#x77;&#x69;&#x6C;&#x6C;&#x69;&#x61;&#x6D;&#x73;&#x40;&#x67;&#x6D;&#x61;&#x69;&#x6C;&#x2E;&#x63;&#x6F;&#x6D;) and he will create unique API credentials for you.

Create a `config/private.yml` file with the following contents:

``` yaml
---
blobstore:
  s3:
    access_key_id:     ACCESS
    secret_access_key: PRIVATE
```

You can now create final releases for everyone to enjoy!

```
bosh create release
# test this dev release
git commit -m "updated sslproxy"
bosh create release --final
git commit -m "creating vXYZ release"
git tag vXYZ
git push origin master --tags
```