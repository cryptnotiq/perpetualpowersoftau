Attestation to response 0020
============================

**Date:** 25 - 28 January 2019

**Name:** Eduardo Antuña

**Location:** N/A

**Device(s):** Personal Intel NUC7CJYH. Intel(R) Celeron(R) J4005 CPU @ 2.00GHz + 8GB RAM + SGX enabled

Challenge hash:

        5bfc6971 5a5f57d2 9170410c 04cfb19d                        
        b079de9d a94f9b81 770d7cfd 6cfb7b14                        
        d05e3268 596722a6 546a40b6 79194ce9                        
        682d3956 23a664a9 a7d836c9 67a41d7d 
       
Challenge file claims, based on the response hash:

        98900cd9 b12a8619 aaa134ee e1a20ec8
        89acc1af ca5a8131 c8b92c47 60f3b6b2
        6132ec96 31c7e735 4547252f 2d578d29
        3fcb91af d797edd1 b28fef9e 7105b5b8

Response hash:

        e9d2416c 15ed22d9 c6b7c2f5 683c21e9
        9409df85 b068b3a1 f0c63984 061e13b2
        1905b533 724ce1c8 7d756409 36706c9c
        0026d811 64e17e1f 7b84e735 392835f6

**Software used:** 

https://github.com/eduadiez/PowersOfTau-SGX/releases/tag/v1.0.0 (https://github.com/eduadiez/PowersOfTau-SGX/tree/b98721c3f1868e1e75b71f6aeb38cd85cbf1af10)

**Response:**

- Blake2b: 
        
        e9d2416c 15ed22d9 c6b7c2f5 683c21e9
        9409df85 b068b3a1 f0c63984 061e13b2
        1905b533 724ce1c8 7d756409 36706c9c
        0026d811 64e17e1f 7b84e735 392835f6

**Entropy sources:** 

A hardware-based pseudorandom generator (PRNG) available in Intel CPUs through the RDRAND instruction on the SGX + A random ethereum block hash 

**Time taken:** 

Around 3 days

**Side channel defenses:** 

I've used a modified version from the original implementation in which the generation of the Keystore occurs within an secure enclave using the Intel SGX technology. 
The rationale behind this is to create the key pair to generate the proof and eliminate the toxic waste inside a secure enclave with protected memory in a way that nobody can access, not even the initiator of the ceremony (unless someone exploits an SGX vulnerability during the process).
It is also possible to get an attestation from Intel with the public key used to generate the proof and the previous hash.

**Postprocessing:**

The compute_constrained_sgx binary generates a quote.bin file when it finishes. With this file it is possible to call the Intel attestation services to verify that the proof was generated according to its standards. 
In this way we can prove - or attest - that the binary wasn't modified, the CPU had the latest security updates and the CPU hadn't enabled the HyperThreading technology, among other things.

This is the result of my attestations qoute:

```
$ curl -i -X POST https://api.trustedservices.intel.com/sgx/attestation/v3/report -H 'Content-Type: application/json' -H 'Ocp-Apim-Subscription-Key: 55aad22ed260486685fab7237d0c7915' -d '{"isvEnclaveQuote":"AgAAAIYLAAAKAAkAAAAAAODpGN28hJIVaOQDutDJ/Ah17UYs7+opWcNky9jY4V16AgIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABQAAAAAAAAADAAAAAAAAAFezT7RffLAzldx+6OtJPKYG0p1OtHfZ92R5aJyM/rvqAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAw56oBvWVB3AC6ZGkHrQLUnWvkTcvZllW8HAzyCD0ZOgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAC9/XWWRh64ji3oG5Bx3J8jX33ExQkpMJhutfOjzaxfTAR1fRt1sRSWNBRrBauzj655gRAkNpHhynhAe9mjB0fiqAIAAHPODYSoCdkrDg0D8aKPt4Pk6Ugpg/LNo96cSVL+1Ss534v8bkCwe/VWRsN+CuUYLyd9ONCH2N7KIioJYVUV/ENcaIhJFDPxSSbvR+5D1DWx+hWBrDGpznuZN0y14bbTGeaSiy4cNXylni/2bEz5zTMY2kpPzYNFU5RA3F5amzG063KQfSIm52dm1VLuUHv7XYpZw4Yk9Z6Nw/ADq3lMYpgXDaLzkjUPagZb0kGN2UsJTgUcjyXtdS8A6678KORQRAkV9hX6biJOFGYnHtuSZmLEr12RsmYDd1tkt1sz0i2NFP7so+z3EuxIofjGVndvMULZdl0ScvEF3V9NUS6TrHRBaEjHBZ5gDEV4XrKFfc/nfVabDJ4Rm4p2MW4nvn8aCuCXm7pNfsQ7H/OYdGgBAABFAmpo5rzuLvDcw77pKc9O1TT9QtN+CwuACen2+DTirm+kCV6fWmEvUx4HfvBN6M+4ViKRW0reIq7DRd0L/EBGNaqxkBPxJSNuuNHbfMveiRMzoLktwApzlU/l6sKpbsOy0woPLSRrISVgVoz+GkQErdhqvrW17sKVZke/9Jna5Z7Q6PfDd/IXzxdfXOf1QpIz8LaM4lk1V8yPR/cxFbU9WTkfUG1Oib9g0pmRg5REOUrlMCJ7YlGTZuCsqoFYtJqIvSRe4+6g5ywbk8NTlFZN57nEBRqypj8NRKm25mb0j8XNitiCFUoKJ3xW1e2TUjrqTKbdHugQIHs6L95MyUbKsMtZ3ffhJTTm1FFBfW/++dXNKKO2ZBbO3471YhmpFcPWEEVpHYArooe5d3L9Rd2d/MFPPeKJJcGqxhkXgn6GXntjKeGUSISOHnU0iK2CoyiLTgn6uA57kQtcrmIq42S56Ra6BzWkIZ9+80Fc0s7fJXC2qlFY6Hrr"}'
HTTP/1.1 100 Continue

HTTP/1.1 200 OK
Content-Length: 731
Content-Type: application/json
Request-ID: 8e6acbc3a2494b8992ebb01e02fad591
X-IASReport-Signature: G9mOXkfiIbK3m5vER7NgY1vZysvPMJmVjFGzdEKcKDdL+QnqNLUBXnqax4jL9seOLJBoAlGF2pDvXMsN2IUvGahX9B1JZCHU7n3LVNZbJxJY7iFBbeNGcJeFT/6Fl4IjnqjSGl7R9dT6w+KP7HA7hayZvVXbq3HOsb4HhlSucNPEIcE5/W1vfScOybaQuqDXuL5soO2Gmhu24LS1Yn/I0N7L63scZdkshIGfjaemCf3CjZyuvv4ErB+AFYPN6wKyYSlKCRti2zUxn2PbNMOvduQeGt8jlxg31/lZxvLqi2xu5H9sTKKLirkEkEHnhko0nLR6g08Xx1fUtK3PCEIOBQ==
X-IASReport-Signing-Certificate: -----BEGIN%20CERTIFICATE-----%0AMIIEoTCCAwmgAwIBAgIJANEHdl0yo7CWMA0GCSqGSIb3DQEBCwUAMH4xCzAJBgNV%0ABAYTAlVTMQswCQYDVQQIDAJDQTEUMBIGA1UEBwwLU2FudGEgQ2xhcmExGjAYBgNV%0ABAoMEUludGVsIENvcnBvcmF0aW9uMTAwLgYDVQQDDCdJbnRlbCBTR1ggQXR0ZXN0%0AYXRpb24gUmVwb3J0IFNpZ25pbmcgQ0EwHhcNMTYxMTIyMDkzNjU4WhcNMjYxMTIw%0AMDkzNjU4WjB7MQswCQYDVQQGEwJVUzELMAkGA1UECAwCQ0ExFDASBgNVBAcMC1Nh%0AbnRhIENsYXJhMRowGAYDVQQKDBFJbnRlbCBDb3Jwb3JhdGlvbjEtMCsGA1UEAwwk%0ASW50ZWwgU0dYIEF0dGVzdGF0aW9uIFJlcG9ydCBTaWduaW5nMIIBIjANBgkqhkiG%0A9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqXot4OZuphR8nudFrAFiaGxxkgma/Es/BA%2Bt%0AbeCTUR106AL1ENcWA4FX3K%2BE9BBL0/7X5rj5nIgX/R/1ubhkKWw9gfqPG3KeAtId%0Acv/uTO1yXv50vqaPvE1CRChvzdS/ZEBqQ5oVvLTPZ3VEicQjlytKgN9cLnxbwtuv%0ALUK7eyRPfJW/ksddOzP8VBBniolYnRCD2jrMRZ8nBM2ZWYwnXnwYeOAHV%2BW9tOhA%0AImwRwKF/95yAsVwd21ryHMJBcGH70qLagZ7Ttyt%2B%2BqO/6%2BKAXJuKwZqjRlEtSEz8%0AgZQeFfVYgcwSfo96oSMAzVr7V0L6HSDLRnpb6xxmbPdqNol4tQIDAQABo4GkMIGh%0AMB8GA1UdIwQYMBaAFHhDe3amfrzQr35CN%2Bs1fDuHAVE8MA4GA1UdDwEB/wQEAwIG%0AwDAMBgNVHRMBAf8EAjAAMGAGA1UdHwRZMFcwVaBToFGGT2h0dHA6Ly90cnVzdGVk%0Ac2VydmljZXMuaW50ZWwuY29tL2NvbnRlbnQvQ1JML1NHWC9BdHRlc3RhdGlvblJl%0AcG9ydFNpZ25pbmdDQS5jcmwwDQYJKoZIhvcNAQELBQADggGBAGcIthtcK9IVRz4r%0ARq%2BZKE%2B7k50/OxUsmW8aavOzKb0iCx07YQ9rzi5nU73tME2yGRLzhSViFs/LpFa9%0AlpQL6JL1aQwmDR74TxYGBAIi5f4I5TJoCCEqRHz91kpG6Uvyn2tLmnIdJbPE4vYv%0AWLrtXXfFBSSPD4Afn7%2B3/XUggAlc7oCTizOfbbtOFlYA4g5KcYgS1J2ZAeMQqbUd%0AZseZCcaZZZn65tdqee8UXZlDvx0%2BNdO0LR%2B5pFy%2BjuM0wWbu59MvzcmTXbjsi7HY%0A6zd53Yq5K244fwFHRQ8eOB0IWB%2B4PfM7FeAApZvlfqlKOlLcZL2uyVmzRkyR5yW7%0A2uo9mehX44CiPJ2fse9Y6eQtcfEhMPkmHXI01sN%2BKwPbpA39%2BxOsStjhP9N1Y1a2%0AtQAVo%2ByVgLgV2Hws73Fc0o3wC78qPEA%2Bv2aRs/Be3ZFDgDyghc/1fgU%2B7C%2BP6kbq%0Ad4poyb6IW8KCJbxfMJvkordNOgOUUxndPHEi/tb/U7uLjLOgPA%3D%3D%0A-----END%20CERTIFICATE-----%0A-----BEGIN%20CERTIFICATE-----%0AMIIFSzCCA7OgAwIBAgIJANEHdl0yo7CUMA0GCSqGSIb3DQEBCwUAMH4xCzAJBgNV%0ABAYTAlVTMQswCQYDVQQIDAJDQTEUMBIGA1UEBwwLU2FudGEgQ2xhcmExGjAYBgNV%0ABAoMEUludGVsIENvcnBvcmF0aW9uMTAwLgYDVQQDDCdJbnRlbCBTR1ggQXR0ZXN0%0AYXRpb24gUmVwb3J0IFNpZ25pbmcgQ0EwIBcNMTYxMTE0MTUzNzMxWhgPMjA0OTEy%0AMzEyMzU5NTlaMH4xCzAJBgNVBAYTAlVTMQswCQYDVQQIDAJDQTEUMBIGA1UEBwwL%0AU2FudGEgQ2xhcmExGjAYBgNVBAoMEUludGVsIENvcnBvcmF0aW9uMTAwLgYDVQQD%0ADCdJbnRlbCBTR1ggQXR0ZXN0YXRpb24gUmVwb3J0IFNpZ25pbmcgQ0EwggGiMA0G%0ACSqGSIb3DQEBAQUAA4IBjwAwggGKAoIBgQCfPGR%2BtXc8u1EtJzLA10Feu1Wg%2Bp7e%0ALmSRmeaCHbkQ1TF3Nwl3RmpqXkeGzNLd69QUnWovYyVSndEMyYc3sHecGgfinEeh%0ArgBJSEdsSJ9FpaFdesjsxqzGRa20PYdnnfWcCTvFoulpbFR4VBuXnnVLVzkUvlXT%0AL/TAnd8nIZk0zZkFJ7P5LtePvykkar7LcSQO85wtcQe0R1Raf/sQ6wYKaKmFgCGe%0ANpEJUmg4ktal4qgIAxk%2BQHUxQE42sxViN5mqglB0QJdUot/o9a/V/mMeH8KvOAiQ%0AbyinkNndn%2BBgk5sSV5DFgF0DffVqmVMblt5p3jPtImzBIH0QQrXJq39AT8cRwP5H%0AafuVeLHcDsRp6hol4P%2BZFIhu8mmbI1u0hH3W/0C2BuYXB5PC%2B5izFFh/nP0lc2Lf%0A6rELO9LZdnOhpL1ExFOq9H/B8tPQ84T3Sgb4nAifDabNt/zu6MmCGo5U8lwEFtGM%0ARoOaX4AS%2B909x00lYnmtwsDVWv9vBiJCXRsCAwEAAaOByTCBxjBgBgNVHR8EWTBX%0AMFWgU6BRhk9odHRwOi8vdHJ1c3RlZHNlcnZpY2VzLmludGVsLmNvbS9jb250ZW50%0AL0NSTC9TR1gvQXR0ZXN0YXRpb25SZXBvcnRTaWduaW5nQ0EuY3JsMB0GA1UdDgQW%0ABBR4Q3t2pn680K9%2BQjfrNXw7hwFRPDAfBgNVHSMEGDAWgBR4Q3t2pn680K9%2BQjfr%0ANXw7hwFRPDAOBgNVHQ8BAf8EBAMCAQYwEgYDVR0TAQH/BAgwBgEB/wIBADANBgkq%0AhkiG9w0BAQsFAAOCAYEAeF8tYMXICvQqeXYQITkV2oLJsp6J4JAqJabHWxYJHGir%0AIEqucRiJSSx%2BHjIJEUVaj8E0QjEud6Y5lNmXlcjqRXaCPOqK0eGRz6hi%2BripMtPZ%0AsFNaBwLQVV905SDjAzDzNIDnrcnXyB4gcDFCvwDFKKgLRjOB/WAqgscDUoGq5ZVi%0AzLUzTqiQPmULAQaB9c6Oti6snEFJiCQ67JLyW/E83/frzCmO5Ru6WjU4tmsmy8Ra%0AUd4APK0wZTGtfPXU7w%2BIBdG5Ez0kE1qzxGQaL4gINJ1zMyleDnbuS8UicjJijvqA%0A152Sq049ESDz%2B1rRGc2NVEqh1KaGXmtXvqxXcTB%2BLjy5Bw2ke0v8iGngFBPqCTVB%0A3op5KBG3RjbF6RRSzwzuWfL7QErNC8WEy5yDVARzTA5%2BxmBc388v9Dm21HGfcC8O%0ADD%2BgT9sSpssq0ascmvH49MOgjt1yoysLtdCtJW/9FZpoOypaHx0R%2BmJTLwPXVMrv%0ADaVzWh5aiEx%2BidkSGMnX%0A-----END%20CERTIFICATE-----%0A
Date: Tue, 28 Jan 2020 12:20:20 GMT

{"id":"204955653670173282863145566959457621871","timestamp":"2020-01-28T12:20:20.947848","version":3,"isvEnclaveQuoteStatus":"OK","isvEnclaveQuoteBody":"AgAAAIYLAAAKAAkAAAAAAODpGN28hJIVaOQDutDJ/Ah17UYs7+opWcNky9jY4V16AgIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABQAAAAAAAAADAAAAAAAAAFezT7RffLAzldx+6OtJPKYG0p1OtHfZ92R5aJyM/rvqAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAw56oBvWVB3AC6ZGkHrQLUnWvkTcvZllW8HAzyCD0ZOgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAC9/XWWRh64ji3oG5Bx3J8jX33ExQkpMJhutfOjzaxfTAR1fRt1sRSWNBRrBauzj655gRAkNpHhynhAe9mjB0fi"}
```

**"isvEnclaveQuoteStatus":"OK"**

Parsing the quote
```
./parse_quote.py <(echo "AgAAAIYLAAAKAAkAAAAAAODpGN28hJIVaOQDutDJ/Ah17UYs7+opWcNky9jY4V16AgIAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABQAAAAAAAAADAAAAAAAAAFezT7RffLAzldx+6OtJPKYG0p1OtHfZ92R5aJyM/rvqAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAw56oBvWVB3AC6ZGkHrQLUnWvkTcvZllW8HAzyCD0ZOgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAC9/XWWRh64ji3oG5Bx3J8jX33ExQkpMJhutfOjzaxfTAR1fRt1sRSWNBRrBauzj655gRAkNpHhynhAe9mjB0fiqAIAAHPODYSoCdkrDg0D8aKPt4Pk6Ugpg/LNo96cSVL+1Ss534v8bkCwe/VWRsN+CuUYLyd9ONCH2N7KIioJYVUV/ENcaIhJFDPxSSbvR+5D1DWx+hWBrDGpznuZN0y14bbTGeaSiy4cNXylni/2bEz5zTMY2kpPzYNFU5RA3F5amzG063KQfSIm52dm1VLuUHv7XYpZw4Yk9Z6Nw/ADq3lMYpgXDaLzkjUPagZb0kGN2UsJTgUcjyXtdS8A6678KORQRAkV9hX6biJOFGYnHtuSZmLEr12RsmYDd1tkt1sz0i2NFP7so+z3EuxIofjGVndvMULZdl0ScvEF3V9NUS6TrHRBaEjHBZ5gDEV4XrKFfc/nfVabDJ4Rm4p2MW4nvn8aCuCXm7pNfsQ7H/OYdGgBAABFAmpo5rzuLvDcw77pKc9O1TT9QtN+CwuACen2+DTirm+kCV6fWmEvUx4HfvBN6M+4ViKRW0reIq7DRd0L/EBGNaqxkBPxJSNuuNHbfMveiRMzoLktwApzlU/l6sKpbsOy0woPLSRrISVgVoz+GkQErdhqvrW17sKVZke/9Jna5Z7Q6PfDd/IXzxdfXOf1QpIz8LaM4lk1V8yPR/cxFbU9WTkfUG1Oib9g0pmRg5REOUrlMCJ7YlGTZuCsqoFYtJqIvSRe4+6g5ywbk8NTlFZN57nEBRqypj8NRKm25mb0j8XNitiCFUoKJ3xW1e2TUjrqTKbdHugQIHs6L95MyUbKsMtZ3ffhJTTm1FFBfW/++dXNKKO2ZBbO3471YhmpFcPWEEVpHYArooe5d3L9Rd2d/MFPPeKJJcGqxhkXgn6GXntjKeGUSISOHnU0iK2CoyiLTgn6uA57kQtcrmIq42S56Ra6BzWkIZ9+80Fc0s7fJXC2qlFY6Hrr" | base64 --decode)
               QUOTE
             version	2
           sign_type	0
       epid_group_id	860b0000
             isv_svn	0000
            reserved	090000000000
            basename	e0e918ddbc84921568e403bad0c9fc0875ed462cefea2959c364cbd8d8e15d7a
              report	0202000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000500000000000000030000000000000057b34fb45f7cb03395dc7ee8eb493ca606d29d4eb477d9f76479689c8cfebbea000000000000000000000000000000000000000000000000000000000000000030e7aa01bd6541dc00ba646907ad02d49d6be44dcbd99655bc1c0cf2083d193a00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000bdfd7596461eb88e2de81b9071dc9f235f7dc4c5092930986eb5f3a3cdac5f4c04757d1b75b1149634146b05abb38fae798110243691e1ca78407bd9a30747e2
              siglen	680
              rsaenc	73ce0d84a809d92b0e0d03f1a28fb783e4e9482983f2cda3de9c4952fed52b39df8bfc6e40b07bf55646c37e0ae5182f277d38d087d8deca222a09615515fc435c6888491433f14926ef47ee43d435b1fa1581ac31a9ce7b99374cb5e1b6d319e6928b2e1c357ca59e2ff66c4cf9cd3318da4a4fcd8345539440dc5e5a9b31b4eb72907d2226e76766d552ee507bfb5d8a59c38624f59e8dc3f003ab794c6298170da2f392350f6a065bd2418dd94b094e051c8f25ed752f00ebaefc28e450440915f615fa6e224e1466271edb926662c4af5d91b26603775b64b75b33d22d8d14feeca3ecf712ec48a1f8c656776f3142d9765d1272f105dd5f4d512e93ac74
             keyhash	416848c7059e600c45785eb2857dcfe77d569b0c9e119b8a76316e27be7f1a0a
                  iv	e0979bba4d7ec43b1ff39874
              enclen	360
              sigenc	45026a68e6bcee2ef0dcc3bee929cf4ed534fd42d37e0b0b8009e9f6f834e2ae6fa4095e9f5a612f531e077ef04de8cfb85622915b4ade22aec345dd0bfc404635aab19013f125236eb8d1db7ccbde891333a0b92dc00a73954fe5eac2a96ec3b2d30a0f2d246b212560568cfe1a4404add86abeb5b5eec2956647bff499dae59ed0e8f7c377f217cf175f5ce7f5429233f0b68ce2593557cc8f47f73115b53d59391f506d4e89bf60d29991839444394ae530227b62519366e0acaa8158b49a88bd245ee3eea0e72c1b93c35394564de7b9c4051ab2a63f0d44a9b6e666f48fc5cd8ad882154a0a277c56d5ed93523aea4ca6dd1ee810207b3a2fde4cc946cab0cb59ddf7e12534e6d451417d6ffef9d5cd28a3b66416cedf8ef56219a915c3d61045691d802ba287b97772fd45dd9dfcc14f3de28925c1aac61917827e865e7b6329e19448848e1e753488ad82a3288b4e09fab80e7b910b5cae622ae364b9
           rl_verenc	e916ba07
               n2enc	35a4219f
                 tag	7ef3415cd2cedf2570b6aa5158e87aeb

              REPORT
             cpu_svn	02020000000000000000000000000000
         misc_select	00000000
          attributes	05000000000000000300000000000000
          mr_enclave	57b34fb45f7cb03395dc7ee8eb493ca606d29d4eb477d9f76479689c8cfebbea
           mr_signer	30e7aa01bd6541dc00ba646907ad02d49d6be44dcbd99655bc1c0cf2083d193a
         isv_prod_id	0000
             isv_svn	0000
         report_data	bdfd7596461eb88e2de81b9071dc9f235f7dc4c5092930986eb5f3a3cdac5f4c04757d1b75b1149634146b05abb38fae798110243691e1ca78407bd9a30747e2

          ATTRIBUTES
               debug	False
           mode64bit	True
        provisionkey	False
       einittokenkey	False
```

report_data =
```
bdfd7596461eb88e2de81b9071dc9f235f7dc4c5092930986eb5f3a3cdac5f4c04757d1b75b1149634146b05abb38fae798110243691e1ca78407bd9a30747e2
```

Where the first part of the report_data is the public part of the keypair used to generate the response:
```
$ shasum -a 256 <(xxd -s -768 -ps response | tr -d \\n)
bdfd7596461eb88e2de81b9071dc9f235f7dc4c5092930986eb5f3a3cdac5f4c  /dev/fd/63
```

and the second one is the sha256 of the previous blake2b challenge hash:
```
$ echo -n "5bfc69715a5f57d29170410c04cfb19db079de9da94f9b81770d7cfd6cfb7b14d05e3268596722a6546a40b679194ce9682d395623a664a9a7d836c967a41d7d" | shasum -a 256
04757d1b75b1149634146b05abb38fae798110243691e1ca78407bd9a30747e2  -
```

The conclusion that can be drawn from this is that the generation of this response was generated within an enclave with the indicated public keypair, and this keypair has been used only to build the response starting from the specified challenge hash. Furthermore, we have an Intel attestation that confirms the process happened as explained.