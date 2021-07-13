<https://github.com/ProtonVPN/android-app/issues/63>

“Access to the Hardware Serial Number: necessary?”


From what I gather, the keys for encryptying shared preferences are generated using a password and a salt, with project properties used for both the password and the salt, only the project property for the salt is ultimately replaced with the hardware serial number (or the Android ID).

My knowledge about cryptography is very limited, but my understanding is that the password may be retrieved with dynamic analysis, so I wonder how secure it is to use the serial number as the salt (although it seems to me that, in order for an attacker to exploit this knowledge, they should have already compromised the device).
In any case, the serial number is sensitive information, could the app do without it? Google discourages its use, and is guarding it with more restrictions (see link below).
The app could, for example, have Android Keystore generate the key with which to encrypt shared preferences - such key would be random and accessible only to the ProtonVPN app.

[Best practices for unique identifiers](https://developer.android.com/training/articles/user-data-ids)

[Android Keystore](https://developer.android.com/training/articles/keystore)
