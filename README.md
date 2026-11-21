# NETWORKWALKS — Week 3 Password Cracking Practical Report

**Project:** Cybersecurity & Ethical Hacking — Week 3  
**Module 1:** Password Cracking with John the Ripper (JTR) and Johnny GUI  
**Module 2:** Password Cracking with NetworkWalks Tools  
**Test file:** `My Locked PDF1.pdf`  
**Environment used:** Ubuntu Linux, terminal, Firefox, John the Ripper Jumbo, Johnny GUI  
**Scope:** Authorized training PDF supplied for the NetworkWalks practical exercises.

## 1. Overview

The objective of both modules was to recover the password of the supplied encrypted PDF and verify that the recovered password opens the file. Module 1 used John the Ripper and its Johnny graphical interface. Module 2 used the browser-based NetworkWalks Hash Calculator and Password Cracker. Although the supplied instructions illustrate Windows, these exercises were performed on Ubuntu Linux.

## 2. Objectives

- Obtain and inspect the provided password-protected PDF.
- Extract the PDF's password-verification data into a `$pdf$...` hash representation.
- Configure John the Ripper Jumbo and Johnny GUI to recognize the PDF hash.
- Recover the PDF password using the tools required by each module.
- Confirm the recovered password opens the PDF.
- Document the procedure and results with screenshots.

## 3. Tools and files

| Tool or file | Purpose |
| --- | --- |
| Ubuntu Linux | Practical operating environment |
| John the Ripper Jumbo | Password auditing and PDF hash support |
| Johnny GUI | Graphical interface for John the Ripper |
| Firefox | Access to the web-based hash extraction and dictionary attack tools |
| OnlineHashCrack PDF hash extractor | Extracted PDF-compatible hash for Module 1 |
| NetworkWalks Hash Calculator | Extracted PDF-compatible hash for Module 2 |
| NetworkWalks Password Cracker | Browser-based dictionary attack for Module 2 |
| `hash.txt` | Local text file containing the extracted PDF hash |
| `My Locked PDF1.pdf` | Authorized encrypted test document |

> **Note:** A PDF `$pdf$...` string encodes encryption and password-verification parameters; it is not an ordinary MD5 or SHA-256 checksum of the PDF file.

## 4. Module 1 — Password Cracking with JTR and Johnny

### Step 1: Prepare the locked PDF

The provided `My Locked PDF1.pdf` was obtained for the lab. The file was password-protected, so its contents could not initially be accessed without the correct password.

![Password-protected PDF before recovery](images/pdflocked.png)

### Step 2: Install and configure John the Ripper Jumbo

John the Ripper Jumbo was built on Ubuntu to provide support for the PDF hash format. The successful build was verified using:

```bash
~/john-jumbo/run/john --list=formats | grep -i pdf
```

The command output included `PDF` among the supported formats. Johnny was configured to use the Jumbo executable rather than the limited system-installed core version:

```text
/home/praise/john-jumbo/run/john
```

The following screenshot records the executable configuration screen during setup. It shows the earlier core executable path (`/usr/sbin/john`), which had to be replaced with the Jumbo path above.

![Johnny executable settings during configuration](images/Johnexecutable.png)

### Step 3: Extract the PDF hash

The encrypted PDF was processed with the PDF hash extractor described in the assignment. The extracted value began with `$pdf$4*4*128*...`, identifying it as a PDF-compatible hash representation.

![Extracted PDF hash](images/hash.png)

### Step 4: Save the hash to a text file

The complete extracted hash was saved as a single entry in `~/Documents/hash/hash.txt`. The file was inspected in the terminal to confirm that it started with `$pdf$` and did not include unwanted surrounding characters.

```bash
cat ~/Documents/hash/hash.txt
```

![PDF hash saved in hash.txt](images/hashtxt.png)

### Step 5: Load the hash into Johnny GUI

Johnny was opened, the saved hash file was selected through **Open password file**, and the PDF hash was recognized. The interface identified the format as `PDF`.

![PDF hash loaded into Johnny GUI](images/johnnyGUI.png)

### Step 6: Recover the password

A password recovery attack was completed using the JTR/Johnny workflow. Johnny reported **1 cracked, 0 left**, and displayed the recovered password:

```text
good-luck
```

![Successful password recovery in Johnny](images/crackedpassword.png)

### Step 7: Verify access to the PDF

The recovered password was used to open the protected PDF. The document displayed its congratulatory completion page, confirming that the recovered password worked.

![Unlocked PDF verification](images/pdfunloacked.png)

**Module 1 result:** Successful — the PDF password was recovered and the document opened.

## 5. Module 2 — Password Cracking with NetworkWalks Tools

### Step 1: Open the NetworkWalks Hash Calculator

The NetworkWalks Hash Calculator was opened in Firefox. The **PDF** tab was selected because the **File** tab only produces ordinary file checksums, not the PDF password-verification hash needed for this exercise.

![NetworkWalks Hash Calculator interface](images/NW-hashcalculator.png)

### Step 2: Extract the encrypted PDF hash

`My Locked PDF1.pdf` was selected in the PDF tab. The calculator recognized the document as encrypted and displayed a hash beginning with `$pdf$...`. The full hash was copied for use in the Password Cracker.

![NetworkWalks PDF hash extraction result](images/NW-hash.png)

### Step 3: Open the NetworkWalks Password Cracker

The browser-based Password Cracker was opened. Its interface provided a field for the extracted PDF hash, a built-in password list, and an option to upload a `.txt` wordlist.

![NetworkWalks Password Cracker ready for input](images/NW-passwordcracker.png)

### Step 4: Run the dictionary attack

The PDF hash was pasted into the Password Cracker. An initial test with the built-in list of 100 passwords exhausted the list without a match. A larger wordlist was then used for the successful attempt.

![Password Cracker during dictionary attack](images/hashcracker.png)

### Step 5: Record the successful result

The Password Cracker displayed **PASSWORD CRACKED SUCCESSFULLY** and returned:

```text
good-luck
```

The successful attempt's screenshot shows the tool testing candidates from a list of 3,556 entries and finding a match near the end of the run.

![NetworkWalks successful password recovery](images/NW-crackedpassword.png)

### Step 6: Verify the recovered password

The same password, `good-luck`, was used to open the supplied PDF. The unlocked PDF screenshot in Module 1 also documents the final verification outcome.

**Module 2 result:** Successful — the NetworkWalks tool recovered the PDF password.

## 6. Challenges encountered and resolutions

| Challenge | Resolution |
| --- | --- |
| The system-installed John core build did not support the PDF hash as required. | Built and verified John the Ripper Jumbo, which lists the PDF format. |
| Johnny initially pointed to `/usr/sbin/john`. | Identified the Jumbo executable at `/home/praise/john-jumbo/run/john` for Johnny configuration. |
| Running `./johnny` from the `john-jumbo/src` directory returned “No such file or directory.” | Launched Johnny from its own directory (`~/johnny/johnny`). |
| The NetworkWalks calculator initially displayed MD5/SHA file checksums. | Switched from the **File** tab to the **PDF** tab to extract the correct hash. |
| The first browser-based attack exhausted the 100-password built-in list. | Used a larger wordlist and obtained a match. |

## 7. Results summary

| Requirement | Module 1: JTR / Johnny | Module 2: NetworkWalks |
| --- | --- | --- |
| Encrypted PDF obtained | Completed | Completed |
| PDF hash extracted | Completed | Completed |
| Password recovery performed | Completed | Completed |
| Recovered password | `good-luck` | `good-luck` |
| Password verified by opening PDF | Yes | Yes |
| Overall outcome | **Successful** | **Successful** |

## 8. Key lessons learned

1. A password-protected document can be assessed by extracting the encryption parameters into a format understood by a password-auditing tool.
2. John the Ripper **core** and **Jumbo** support different sets of formats; selecting the correct executable matters.
3. A graphical interface such as Johnny depends on the underlying John executable for the actual work.
4. Dictionary attacks only test passwords included in their candidate lists; a failed wordlist does not prove that a password is uncrackable.
5. Short or predictable passwords are vulnerable when they appear in common wordlists. Strong, unique, longer passwords offer better protection.
6. Password recovery tests must be limited to files and systems the tester owns or is authorized to assess.

## 9. Conclusion

Both Week 3 practical modules were completed successfully on Ubuntu Linux. John the Ripper Jumbo with Johnny GUI and the NetworkWalks browser-based tools recovered the same password, **`good-luck`**, from the authorized encrypted training PDF. Opening the document with the recovered password confirmed the result. The exercise demonstrated the workflow of PDF hash extraction, dictionary-based password recovery, tool configuration, and verification.

## 10. Assignment references

- NetworkWalks, *Week 3 — Project Module 1: Password Cracking with JTR* (provided assignment PDF).
- NetworkWalks, *Week 3 — Project Module 2: Password Cracking with NetworkWalks Tools* (provided assignment PDF).
- [John the Ripper — Openwall](https://www.openwall.com/john/)
- [Johnny GUI documentation](https://openwall.info/wiki/john/johnny)
- [NetworkWalks Hash Calculator](https://networkwalks.com/hash-calculator/)
- [NetworkWalks Password Cracker](https://networkwalks.com/password-cracker/)

---

### GitHub image display note

Keep `REPORT.md` at the repository root and upload the `images/` directory alongside it. The relative paths such as `![Example](images/NW-crackedpassword.png)` will render the screenshots **inside the report** on GitHub. The image files must be committed with exactly these names and capitalization.
