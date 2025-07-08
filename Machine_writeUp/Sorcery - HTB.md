# Enumeration 
# Nmap

```
22/tcp  open  ssh      OpenSSH 9.6p1 Ubuntu 3ubuntu13.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 79:93:55:91:2d:1e:7d:ff:f5:da:d9:8e:68:cb:10:b9 (ECDSA)
|_  256 97:b6:72:9c:39:a9:6c:dc:01:ab:3e:aa:ff:cc:13:4a (ED25519)
443/tcp open  ssl/http nginx 1.27.1
|_http-title: Did not follow redirect to https://sorcery.htb/
| ssl-cert: Subject: commonName=sorcery.htb
| Issuer: commonName=Sorcery Root CA
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-10-31T02:09:11
| Not valid after:  2052-03-18T02:09:11
| MD5:   c294:7d7a:2965:5c32:3dc9:b850:e2e5:0d9a
|_SHA-1: 9d44:6d3d:5fb6:252c:da8b:3dd1:b5a2:aeb3:1e4b:5534
|_http-server-header: nginx/1.27.1
| tls-alpn: 
|   http/1.1
|   http/1.0
|_  http/0.9
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_ssl-date: TLS randomness does not represent time
```



![[Pasted image 20250621134637.png]]

![[Pasted image 20250621135555.png]]


```rust
// ./db/models/product.rs
#[derive(Model, Serialize, Deserialize)]
pub struct Product {
    pub id: String,
    pub name: String,
    pub description: String,
    pub is_authorized: bool,
    pub created_by_id: String,
}

// ./products/get_one.rs
#[get("/<id>")]
pub async fn get_one(guard: RequireClient, id: &str) -> Result<Json<Response>, AppError> {
    let product = match Product::get_by_id(id.to_owned()).await {
        Some(product) => product,
        None => return Err(AppError::NotFound),
    };
```

# Cypher Injection