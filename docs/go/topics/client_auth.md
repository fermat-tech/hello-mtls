In your server's Go file, we pass a TLS stack configuration into the server initalization. The configuration enables strict client certificate verification against all trusted root certificates in the CA pool we create. Finally, we call the server's API to create a HTTPS listener using its own certificate and private key for client-side verification.

```go
// ...

caCert, _ := os.ReadFile("{{ ca_cert }}")
caCertPool := x509.NewCertPool()
caCertPool.AppendCertsFromPEM(caCert)

tlsConfig := &tls.Config{
    ClientCAs: caCertPool,
    ClientAuth: tls.RequireAndVerifyClientCert,
}

server := &http.Server{
    Addr:      ":9443",
    TLSConfig: tlsConfig,
}

server.ListenAndServeTLS("{{ server_cert }}", "{{ server_key }}")

// ...
```
