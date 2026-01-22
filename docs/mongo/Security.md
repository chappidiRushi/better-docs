---

id: mongodb-security
title: Security
sidebar_position: 16
--------------------

# 16. Security

> Authentication, authorization, encryption, and hardening strategies in MongoDB

---

## Authentication

> Verifying client identity before granting access

**Interview points**

* SCRAM is default auth mechanism
* Supports X.509, LDAP, Kerberos
* Per-user credentials

<details>
<summary>Examples (Node.js)</summary>

```js
// Connect using SCRAM authentication
const client = new MongoClient('mongodb://user:pass@host:27017/admin');
await client.connect();
```

</details>

---

## Authorization

> Role-based access control (RBAC)

**Interview points**

* Roles define privileges
* Built-in and custom roles
* Enforced per database and collection

<details>
<summary>Examples (Node.js)</summary>

```js
// Create user with role
await adminDb.command({
  createUser: 'appUser',
  pwd: 'secret',
  roles: [
    { role: 'readWrite', db: 'app' }
  ]
});
```

</details>

---

## TLS

> Encrypting data in transit

**Interview points**

* TLS/SSL required for production
* Prevents MITM attacks
* Client and server certificates

<details>
<summary>Examples (Node.js)</summary>

```js
// TLS-enabled client connection
const client = new MongoClient(uri, {
  tls: true,
  tlsCAFile: '/path/ca.pem',
  tlsCertificateKeyFile: '/path/client.pem'
});

await client.connect();
```

</details>

---

## Encryption at Rest

> Protecting data on disk

**Interview points**

* WiredTiger encryption
* KMIP or local key management
* Transparent to applications

<details>
<summary>Examples (Node.js)</summary>

```js
// Verify encryption status
await adminDb.command({ serverStatus: 1 })
  .then(r => r.encryptionAtRest);
```

</details>

---

## Injection Risks

> Preventing query injection attacks

**Interview points**

* No string-based query parsing
* Risk via unchecked operators
* Validate user input

<details>
<summary>Examples (Node.js)</summary>

```js
// ❌ Dangerous: user-controlled operator
await db.collection('users').find(req.body);

// ✅ Safe: explicit field mapping
await db.collection('users').find({ email: req.body.email });
```

</details>

---

## Network Hardening

> Reducing attack surface

**Interview points**

* Bind to private interfaces
* Use firewalls / security groups
* Disable unused ports

<details>
<summary>Examples (Node.js)</summary>

```js
// Verify bind IPs
await adminDb.command({ getCmdLineOpts: 1 });
```

</details>

---

## Notes

* Security is defense-in-depth
* Defaults are insecure without auth

## Caveats

* Misconfigured roles cause privilege escalation
* TLS misconfiguration breaks clients
* Security requires operational discipline
