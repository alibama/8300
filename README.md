
<img width="623" alt="Screenshot 2023-08-29 at 2 05 35 PM" src="https://github.com/alibama/8300/assets/911386/3eb8501a-69c8-4fb1-8009-44d002022f2f">

Filing taxes should be simpler. 


This is a repository focused on automating crypto finance disclosures for the IRS given thge schema of the 8300 form [https://www.irs.gov/pub/irs-pdf/f8300.pdf](https://www.irs.gov/pub/irs-pdf/f8300.pdf)


* Goal - a sufficiently robust schema to accommodate a majority of transactions.
* Technical spec - use inputs from attestation services, US GAAP labeled XBRL outputs for IRS submission

Current draft of schema:

**Attestation referencing: invoice(req), Passport (opt), Notary (opt), Passed KYC (req)**

[https://sepolia.easscan.org/schema/view/0x58f602a17cf7f128f19ce7bf2ce6be681d7383b6d3dbc5e4edabecf2b011fd8d](https://sepolia.easscan.org/schema/view/0x58f602a17cf7f128f19ce7bf2ce6be681d7383b6d3dbc5e4edabecf2b011fd8d)

Use [https://docs.attest.sh/docs/tutorials/private-data-attestations](https://docs.attest.sh/docs/tutorials/private-data-attestations) for content that can be private

**Breakdown of referenced schemas:**

** *Invoice attestation reference:**

[https://easscan.org/schema/view/0x34b5dd4d3c05ab7cf6268e02df8927c767a0b9fcc108f8f5ee224e3f54b788a1](https://easscan.org/schema/view/0x34b5dd4d3c05ab7cf6268e02df8927c767a0b9fcc108f8f5ee224e3f54b788a1) 

string serviceProvider,(req 
string billedTo,(req)
uint24 invoiceNumber, (opt)
string serviceDescription,(opt)
string amountDue,(req)
address paymentAddress,(req)
string issuedDate,(req)
string dueDate(opt)

**Passport attestation reference:**

[https://easscan.org/schema/view/0xf4ff1791c06f4830fcb5f71fcbaa26cfee5c21a7f3114a28ea3d835ae22b8a85](https://easscan.org/schema/view/0xf4ff1791c06f4830fcb5f71fcbaa26cfee5c21a7f3114a28ea3d835ae22b8a85)

bytes32 passportHash,
uint64 expirationDate

**Digital Notary attestation reference**: zk hash of doc?  

[https://easscan.org/schema/view/0xf5e3c5dd9ed54b59bdacceb0def073061e680939f7cff07d9521dc3e87336111](https://easscan.org/schema/view/0xf5e3c5dd9ed54b59bdacceb0def073061e680939f7cff07d9521dc3e87336111)

bytes32 documentHash,
bytes32 notaryCertificate,
uint64 notarizationTime

***Passed KYC attestation reference:**

[https://easscan.org/schema/view/0xc8c8be26e2082ff59e0f1725b2b85322adc80648c7c4084441ba67ee46ad0127](https://easscan.org/schema/view/0xc8c8be26e2082ff59e0f1725b2b85322adc80648c7c4084441ba67ee46ad0127)

bool hasPassedKYC

Zero knowledge for security and simplicity 



* Entity Receiving Cash:
    * Store identifying business information on-chain
    * Use ZKPs to prove business details like name, address, EIN match without revealing
* Person Providing Cash:
    * Store hash of PII like SSN, driver's license on-chain
    * Use ZKPs to prove PII data matches hashes without revealing actual info
* Cash Transaction Details:
    * Store non-PII details on-chain - date, amount, type of transaction
    * Use ZKPs to prove user verified PII for transaction without sharing sensitive info
* ZKPs generated using client-side SDK:
    * SDK allows creating ZKPs for required PII data
    * Proves data validity without sharing sensitive personal details
    * ZKPs stored on-chain tied to attestation transaction
* Verification process:
    * Authorized auditors can verify attestation ZKPs on-chain
    * Confirms validity of cash transaction details and linked PII
    * Auditor does not see actual PII data

This allows complying with IRS reporting of cash transactions while protecting personal privacy. The attestation schema leverages Ethereum for immutable record keeping and ZKPs for selective disclosure.
