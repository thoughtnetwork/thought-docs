# Retrieve your Thought wallet private key on your mobile device
 
1. Choose your wallet -> Click the top-right icon -> Backup -> Read the warning then click Got it-> Read the warning message click I understand.
2. Choose your wallet -> Click the top-right icon -> Export Wallet -> File/Text -> Set up a password -> Copy to clipboard.
3. Then, go to https://bitwiseshiftleft.github.io/sjcl/demo/.  
4. Open your file and copy the entire text.
5. Paste the text in Ciphertext text area.  
6. Enter your password on the Password input. (This password is the one that you will enter in mobile wallet) . Then click “decrypt.”   In the Plaintext text area you will now find the xPrivKey.(Your 12 word private key is listed after “mnemonic”:)

You can then import your privkey into Thought-qt wallet with importprivkey cli or console.
