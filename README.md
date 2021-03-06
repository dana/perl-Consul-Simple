# NAME

Consul::Simple - Easy access to Consul (http://www.consul.io/)

# SYNOPSIS

    my $c = Consul::Simple->new();
    $c->KVPut('foo/bar', 'something');
    my @recs = $c->KVGet('foo/bar');
    #@recs = [
    #      {
    #        'Value' => 'one',
    #        'LockIndex' => 0,
    #        'CreateIndex' => 4,
    #        'Flags' => 0,
    #        'ModifyIndex' => 4,
    #        'Key' => 'foo/bar'
    #      }
    #    ];

    $c->KVPut('foo/fuz', { something => 'rich' });
    my @recs = $c->KVGet('foo/bar', recurse => 1);
    #@recs = [
    #      {
    #        'Value' => 'one',
    #        'LockIndex' => 0,
    #        'CreateIndex' => 4,
    #        'Flags' => 0,
    #        'ModifyIndex' => 4,
    #        'Key' => 'foo/bar'
    #      },
    #      {
    #        'Value' => {
    #                     'something' => 'rich'
    #                   },
    #        'LockIndex' => 0,
    #        'CreateIndex' => 5,
    #        'Flags' => 0,
    #        'ModifyIndex' => 5,
    #        'Key' => 'foo/fuz'
    #      }
    #    ];
    $c->KVDelete('foo/bar');

# DESCRIPTION

Simple interface to Consul (http://www.consul.io/)

# METHODS/CONSTRUCTOR

## new(%options)

Your basic constructor.  It does not take any external action, so it can not
fail.

- kv\_prefix (optional)

    Adds this prefix to all key/value operations, essentially giving you a
    'namespace' inside of Consul, for the life of this object.  Defaults to
    '/'.

- proto (optional)

    Defaults to 'http'.

- consul\_server (optional)

    Defaults to 'localhost'.

- ssl\_opts (optional)

    Defaults to nothing.  Passed into LWP::UserAgent.  For instance, if you
    need to do https operations against a bogus cert:
     ssl\_opts => { SSL\_verify\_mode => 'SSL\_VERIFY\_NONE' }

- retries (optional)

    Defaults to 5.  The number of times to retry a given operation before
    giving up.

- timeout (optional)

    Defaults to 2 seconds.  How long to wait for a given operation to timeout.

- consul\_port (optional)

    Defaults to 8500.  Where the consul http API is listening.

## KVPut($key, $value, %options)

This calls the Consul HTTP PUT method to set a value in the key/value store.

- key (required)

    This is the key, optionally prefixed by kv\_prefix in the constructor.  No
    encoding is done; this is passed to Consul as is.

- value (required)

    This is the value to be PUT with the associated key.  If it is a simple
    scalar, it is passed to Consul as is.  If it is a reference of any kind,
    it is JSON encoded before being sent to Consul.

## KVGet($key, %options)

This calls the Consul HTTP GET method to read a set of records from the
key/value store.  It returns an array of records returned by the API, if any.
The data that was previous PUT is found in the Value field of each record.

- key (required)

    This is the key, optionally prefixed by kv\_prefix in the constructor.  No
    encoding is done; this is passed to Consul as is.

- recurse (optional)

    Causes the ?recurse flag to be sent along, which will cause Consul to return
    all of the records 'below' the passed key.

## KVDelete($key, %options)

This calls the Consul HTTP DELETE method to delete a value or set of values
from the key/value store.

- key (required)

    This is the key, optionally prefixed by kv\_prefix in the constructor.  No
    encoding is done; this is passed to Consul as is.

- recurse (optional)

    Causes the ?recurse flag to be sent along, which will cause Consul to delete
    all of the records 'below' the passed key.

# TODO

Tons.

Actually and correctly test the DELETE functionality.

Implement recurse DELETE.

Add the tons of other Consul features for the KV API.

Add the rest of the non-KV Consul features.

# BUGS

None known.

# COPYRIGHT

Copyright (c) 2014 Dana M. Diederich. All Rights Reserved.

# AUTHOR

Dana M. Diederich <dana@realms.org>
