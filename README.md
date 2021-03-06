Description
===========

Installs and configures Kerberos version 5 authentication modules
on RedHat and Debian family systems.

Requirements
============

Requires some PAM configuration script such as pam-auth-update on Debian
family systems, or authconfig on Redhat family systems.  Best effort is
made to use one of these two tools based on detected platform.

You can override krb5['authconfig'] with an execute command, as a string.
Which should configure PAM to use Kerberos on other systems.

You really need to have time synchronized within 5 minutes of your domain
controllers, or key distribution centers.  Therefore the recipe depends
on the Opscode NTP cookbook.  If you have another method of keeping accurate
clocks, change the metadata according to your needs.

Attributes
==========

 * krb5['packages'] - Packages and libraries needed for Kerberos v5 authentication, detected for Redhat/Debian family systems.
 * krb5['authconfig'] - Configuration script for PAM, detected for RedHat and Debian family systems.
 * krb5['default\_realm'] - The default realm, defaults to OHAI's domain attribute.
 * krb5['realms'] - Array of all realms, including the default.  Defaults to OHAI's domain attribute.
 * krb5['default\_realm\_kdcs'] - Array of Kerberos servers, this is optional, and default empty.  
 * krb5['lookup\_kdcs'] - Set to true if you have SRV records for KDC discovery.  Default is true.

Usage
=====

Here are two example roles to be used with this recipe.  The first, is
a single realm configuration, using the OHAI domain attribute for the realm.

```
name "krb5_domain"
description "Configures Kerberos 5 Authentication for domain realm"
override_attributes "krb5" => {
  "default_realm_kdcs" => [
    "kdc1.example.com",
    "kdc2.example.com",
    "kdc3.example.com"
  ]
}
run_list "recipe[krb5]"
```

The second example is a role for multiple Kerberos realms.


```
name "krb5_multirealm"
description "Configures Kerberos 5 Authentication for example.com and example.org realm"
override_attributes "krb5" => {
  "default_realm" = > "example.com",
  "realms" => [ 
    "example.com",
    "example.org"
  ],
  "default_realm_kdcs" => [
    "kdc1.example.com",
    "kdc2.example.com",
    "kdc3.example.com"
  ],
  "lookup_kdcs" => "true"
}
run_list "recipe[krb5]"
```
