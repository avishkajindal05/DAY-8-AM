-------- -------- ---------- ------------------ ---------------- -------------------
PART D:
Exact Prompt:
Write a Python program that validates an Indian PAN card number format using if-else conditions.
PAN format: 5 uppercase letters, 4 digits, 1 uppercase letter (e.g., ABCDE1234F).
The 4th character indicates the type of taxpayer.
----
AI generated code:
# PAN Validation Program (Using if-else only)

pan = input("Enter PAN number: ")

# Step 1: Check length
if len(pan) != 10:
    print("Invalid PAN: Length must be exactly 10 characters.")

else:
    # Step 2: Check first 5 characters are uppercase letters
    first_part = pan[0:5]
    if not first_part.isalpha() or not first_part.isupper():
        print("Invalid PAN: First 5 characters must be uppercase letters.")
    
    else:
        # Step 3: Check next 4 characters are digits
        middle_part = pan[5:9]
        if not middle_part.isdigit():
            print("Invalid PAN: Characters 6 to 9 must be digits.")
        
        else:
            # Step 4: Check last character is uppercase letter
            last_part = pan[9]
            if not last_part.isalpha() or not last_part.isupper():
                print("Invalid PAN: Last character must be an uppercase letter.")
            
            else:
                # Step 5: Identify taxpayer type from 4th character
                taxpayer_type = pan[3]

                if taxpayer_type == 'P':
                    print("Valid PAN - Taxpayer Type: Individual")
                elif taxpayer_type == 'C':
                    print("Valid PAN - Taxpayer Type: Company")
                elif taxpayer_type == 'H':
                    print("Valid PAN - Taxpayer Type: HUF")
                elif taxpayer_type == 'F':
                    print("Valid PAN - Taxpayer Type: Firm")
                elif taxpayer_type == 'A':
                    print("Valid PAN - Taxpayer Type: AOP")
                elif taxpayer_type == 'T':
                    print("Valid PAN - Taxpayer Type: Trust")
                elif taxpayer_type == 'B':
                    print("Valid PAN - Taxpayer Type: BOI")
                elif taxpayer_type == 'L':
                    print("Valid PAN - Taxpayer Type: Local Authority")
                elif taxpayer_type == 'J':
                    print("Valid PAN - Taxpayer Type: Artificial Juridical Person")
                elif taxpayer_type == 'G':
                    print("Valid PAN - Taxpayer Type: Government")
                else:
                    print("Valid PAN format, but unknown taxpayer type.")
----

Critical evaluation:

Are all positions validated correctly?
-> No—critical validation gaps exist:
| Position | Requirement                                      | Validation Status              | Issue                                                                                                                                             |
| -------- | ------------------------------------------------ | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1–3      | Uppercase letters (specifically alphabetic)      | **Partially validated**        | Only checks that first 5 are uppercase letters; positions 1–3 not isolated                                                                        |
| 4        | Taxpayer type indicator (specific valid letters) | **Not validated structurally** | Only used for classification after format validation; invalid 4th character (e.g., 'X') passes as "unknown taxpayer type" rather than invalid PAN |

Structural Issue: The 4th character has specific valid values (P, C, H, F, A, T, B, L, J, G). The current implementation treats invalid 4th characters as "unknown taxpayer type" rather than rejecting them as invalid PAN format. Per Income Tax Department specifications, the 4th character must be one of the defined type codes.
---
Are edge cases handled?
-> Inadequately. Critical edge cases are unaddressed:
| Edge Case                                         | Current Behavior                       | Expected Behavior                  |
| ------------------------------------------------- | -------------------------------------- | ---------------------------------- |
| `None` input                                      | `TypeError` (unhandled)                | Should be caught or prevented      |
| Non-string input (e.g., integer)                  | `AttributeError` on `.isalpha()`       | Should validate input type         |
| Unicode uppercase letters (e.g., 'Á')             | Accepted by `.isupper()`               | Should reject—only ASCII A-Z valid |
| Whitespace padding (e.g., `" ABCDE1234F "`)       | Length check fails or validation fails | Should strip or explicitly reject  |

Missing Safeguards:
No input sanitization (stripping whitespace)
No type checking before string methods
No ASCII-specific validation (.isupper() accepts non-ASCII uppercase)
---
Is the approach correct (character-by-character vs regex)?
-> The character-by-character approach is acceptable but suboptimal:
| Aspect             | Current Approach                                | Regex Alternative                      | Assessment     |
| ------------------ | ----------------------------------------------- | -------------------------------------- | -------------- |
| Readability        | Deeply nested `if-else` (arrow anti-pattern)    | Single compact pattern                 | Regex superior |
| Maintainability    | Difficult to modify; validation logic scattered | Centralized pattern                    | Regex superior |
| Performance        | Negligible difference for single validation     | Negligible difference                  | Equivalent     |
| Specificity        | Cannot easily enforce valid 4th character set   | Character class `[PCFHATBLJG]` precise | Regex superior |
| Pythonic adherence | Violates "flat is better than nested"           | Clean, declarative                     | Regex superior |

---

Is the code Pythonic?
-> No—multiple violations of Pythonic principles:
| Principle                              | Violation                               | Current State                     | Pythonic Alternative           |
| -------------------------------------- | --------------------------------------- | --------------------------------- | ------------------------------ |
| **"Flat is better than nested"**       | Deep nesting (4 levels)                 | Arrow anti-pattern                | Early returns or guard clauses |
| **"Explicit is better than implicit"** | Taxpayer type mapping                   | Long `if-elif` chain              | Dictionary mapping             |
| **DRY (Don't Repeat Yourself)**        | Repeated validation logic               | `.isalpha() and .isupper()` twice | Helper function or regex       |
| **Single Responsibility**              | Validation + classification intertwined | Single function handles both      | Separate concerns              |

-------------------------------------------

Write your improved version based on the critique:
->
import re


def validate_pan(pan):
    # Input sanitization and type validation
    if pan is None:
        return False, "Invalid PAN: Input cannot be None"
    
    if not isinstance(pan, str):
        return False, f"Invalid PAN: Expected string, got {type(pan).__name__}"
    
    # Strip whitespace and validate length
    pan = pan.strip()
    
    if len(pan) != 10:
        return False, f"Invalid PAN: Length must be exactly 10 characters (got {len(pan)})"

    pattern = r'^[A-Z]{3}[PCFHATBLJG][A-Z]\d{4}[A-Z]$'
    
    if not re.match(pattern, pan):
        # Provide specific feedback for common failures
        if not pan[:3].isalpha() or not pan[:3].isupper():
            return False, "Invalid PAN: First 3 characters must be uppercase letters A-Z"
        
        if pan[3] not in 'PCFHATBLJG':
            return False, f"Invalid PAN: 4th character '{pan[3]}' is not a valid taxpayer type code"
        
        if not pan[4].isalpha() or not pan[4].isupper():
            return False, "Invalid PAN: 5th character must be an uppercase letter A-Z"
        
        if not pan[5:9].isdigit():
            return False, "Invalid PAN: Characters 6-9 must be digits 0-9"
        
        if not pan[9].isalpha() or not pan[9].isupper():
            return False, "Invalid PAN: Last character must be an uppercase letter A-Z"
        
        return False, "Invalid PAN: Format violation detected"
    
    # Taxpayer type classification
    TAXPAYER_TYPES = {
        'P': "Individual",
        'C': "Company",
        'H': "Hindu Undivided Family (HUF)",
        'F': "Firm",
        'A': "Association of Persons (AOP)",
        'T': "Trust",
        'B': "Body of Individuals (BOI)",
        'L': "Local Authority",
        'J': "Artificial Juridical Person",
        'G': "Government"
    }
    
    taxpayer_code = pan[3]
    taxpayer_type = TAXPAYER_TYPES[taxpayer_code]
    
    return True, f"Valid PAN - Taxpayer Type: {taxpayer_type}"


def main():
    """Interactive PAN validation with comprehensive error handling."""
    print("=" * 50)
    print("PAN VALIDATION SYSTEM")
    print("=" * 50)
    print("Format: AAA[A-Z]9999A (3 letters + Type + 1 letter + 4 digits + 1 letter)")
    print("Valid Type Codes: P, C, H, F, A, T, B, L, J, G")
    print("-" * 50)
    
    try:
        user_input = input("\nEnter PAN number: ")
        is_valid, message = validate_pan(user_input)
        
        status = "✓ SUCCESS" if is_valid else "✗ FAILED"
        print(f"\n{status}: {message}")
        
        # Additional metadata for valid PANs
        if is_valid:
            pan = user_input.strip().upper()
            print(f"  Input: {pan}")
            print(f"  Type Code: {pan[3]}")
            
    except KeyboardInterrupt:
        print("\n\nValidation cancelled by user.")
    except Exception as e:
        print(f"\nUnexpected error: {e}")


if __name__ == "__main__":
    main()

-------------------------------------------------------------------------------------