# Retrieve your Thought wallet private key on your mobile device
 
1. Choose your wallet -> Click the top-right icon -> Backup -> Read the warning then click Got it-> Read the warning message click I understand.
2. Choose your wallet -> Click the top-right icon -> Export Wallet -> File/Text -> Set up a password -> Copy to clipboard.
3. Then, go to https://bitwiseshiftleft.github.io/sjcl/demo/.  
4. Open your file and copy the entire text.
5. Paste the text in Ciphertext text area.  
6. Enter your password on the Password input. (This password is the one that you will enter in mobile wallet) . Then click “decrypt.”   In the Plaintext text area you will now find the xPrivKey.(Your 12 word private key is listed after “mnemonic”:)
7. Download bip39 tool from https://github.com/thoughtnetwork/bip39/releases/tag/0.3.13 and open file locally with browser
8. Copy 12 word mnemonic to BIP39 Mnemonic field
9. Select THT - Thought under Coin
10. Scroll to Derived Addresses to find your PrivKey
11. Import PrivKey into Thought-qt/Thoughtd
