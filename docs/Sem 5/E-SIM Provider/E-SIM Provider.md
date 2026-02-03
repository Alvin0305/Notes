#### Security
- Backend should be configured with an SSL/TLS certificate so that the communication b/w app and backend will be encrypted
- if backend is ts, we can use class-validator to prevent sql injections
- rate limit can be set to /send-otp and /login requests
- check vulnerabilities in npm packages using npm audit
- build the app using the following command to prevent reverse engineering
```bash
flutter build appbundle --obfuscate --split-debug-info=<path>
```
#### Payment gateway
- Stripe
- flutter_stripe
#### Flow of payment
1. User selects a plan in the Flutter app.
2. The app tells your backend: "This user wants to buy this plan for $10."
3. Your backend creates a "Payment Intent" with Stripe's API. Stripe returns a client_secret for this transaction.
4. Your backend sends this client_secret (and only this) back to the Flutter app.
5. The Flutter app uses the official Stripe SDK and the client_secret to display the native payment sheet (where the user enters card details).
6. The card details go directly from the user's device to Stripe's servers, bypassing your backend completely.
7. Stripe processes the payment and notifies the Flutter app of success or failure.
8. **Crucially:** The app then notifies your backend that the payment was successful. The backend **must verify this** by listening to a **webhook** from Stripe. This prevents a user from tampering with the app to claim a successful payment. Only after your backend receives and verifies the webhook from Stripe should you provision the e-SIM.
#### Things to keep in mind
- use flutter_secure_storage for keeping the sensitive data like authentication tokens...
- use flutter_dotenv for using .env files to keep the environment variables
- For api calls, we can use http or dio. dio is more powerful
- give error messages for all possible errors
#### Authentication
- we can use Twilio to send OTP via SMS
	- /api/auth/send-otp
	- /api/auth/verify-otp : phone number + entered OTP -> JWT token
- Use JWT token in all the requests
#### Doubts
- should we handle multiple language support
- should we handle multiple currency representation based on location