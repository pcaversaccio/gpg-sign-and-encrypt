# Sign and Encrypt an Email From the `gpg` CLI

This guide explains how to sign and encrypt an email using the `gpg` (GNU Privacy Guard) tool from the command line. It covers the prerequisites, steps to sign and encrypt the email, and how to send it via email.

## Prerequisites

1. **Install GPG:** You need to have `gpg` installed on your system. Install it using the appropriate commands for your operating system:

   - **Linux** (Debian/Ubuntu-based):

   ```bash
   sudo apt install gnupg
   ```

   - **macOS** (with [Homebrew](https://brew.sh)):

   ```bash
   brew install gnupg
   ```

   - **Windows**: Download and install Gpg4win from [gpg4win.org](https://www.gpg4win.org).

2. **Generate a GPG key pair:** If you don't already have a GPG key pair, generate one using:

```bash
gpg --full-generate-key
```

## Steps to Sign and Encrypt an Email

### 1. Write Your Email to a Text File

Create a text file ([`email.txt`](./email.txt)) with your email content:

```txt
To: recipient@example.com
Subject: My Signed and Encrypted Email

Hello,

this is a test email signed and encrypted with GPG.

Best,
Your Name
```

### 2. Sign the Email Using GPG

To sign the email, use the following command:

```bash
gpg --clearsign email.txt
```

> Alternatively, you can specify the key by using the email address or user ID associated with the key: `gpg --clearsign -u "your.email@example.com" email.txt` or `gpg --clearsign -u 7C3B4B4B7725111F email.txt`.

This will generate a signed version of the email called `email.txt.asc` with the GPG signature included. The content will look like this:

```txt
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA256

To: recipient@example.com
Subject: My Signed and Encrypted Email

Hello,

this is a test email signed and encrypted with GPG.

Best,
Your Name
-----BEGIN PGP SIGNATURE-----
...
-----END PGP SIGNATURE-----
```

### 3. Encrypt the Signed Email

Now encrypt the signed message using the recipient's public key:

```bash
gpg --encrypt --recipient recipient@email.com email.txt.asc
```

This will generate a file called `email.txt.gpg` (in raw binary data).

> To sign and encrypt in one step, you can use: `gpg --sign --encrypt --recipient recipient@email.com email.txt`.

### 4. Send the Signed Email

Attach the `email.txt.gpg` file to your email in Outlook or any other email client. The body of your email can explain that you've attached an encrypted message.

## Optional: Inline Signature

If you prefer to embed the signature directly within the email body, use this command:

```bash
gpg --sign --armor email.txt
```

This will create an `email.txt.asc` file that contains both the signed message and the signature. To combine it with the encryption step, you can use:

```bash
gpg -a --sign --encrypt --recipient recipient@email.com email.txt
```

> [!TIP]
> Use [ASCII Armor](https://openpgp.dev/book/armor.html) when:
>
> 1. Sending encrypted content in the body of an email,
> 2. Posting encrypted messages on text-based platforms,
> 3. You need the content to be viewable/editable in a text editor,
> 4. Compatibility is a concern (some systems handle ASCII better).

## Verify a Signed Email

If you receive a GPG-signed email and want to verify the signature, you can run the following command:

```bash
gpg --verify email.txt.asc
```

This command will check the signature and indicate if the email was signed by a valid key.

## Decrypt a Signed and Encrypted Email

To decrypt an encrypted email, use one of the following commands based on the file format:

```bash
gpg --decrypt email.txt.gpg
```

or if you used ASCII armor:

```bash
gpg --decrypt email.txt.asc
```

> [!NOTE]
> Please note that `gpg --decrypt` will verify the signature automatically during decryption if the file was signed.
