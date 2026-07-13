# User-Server Interaction: The Role of Cookies

This section explains why HTTP is stateless and how Cookies solve the identification challenge.

## 1. The Stateless Dilemma
* **The "Pro":** HTTP servers are stateless by design to maximize performance, allowing them to handle thousands of concurrent TCP connections without storing complex user states in RAM.
* **The "Con":** Servers cannot naturally recognize returning users, preventing personalization or session management.

## 2. The Engineering Solution: Cookies (RFC 6265)
* **Definition:** A piece of data stored by the browser to track user identity.
* **Function:** Cookies bridge the gap between "Stateless HTTP" and "Stateful User Experience."
* **Use Cases:**
    * **Access Control:** Verifying user login status.
    * **Personalization:** Maintaining shopping carts or user preferences.

## 3. Engineering Reality
* Cookies are the standard mechanism used by almost every modern commercial website to maintain state across independent HTTP requests.
