# Implementing Photo Selfie Verification With Nigeria BVN

Appruve makes it easy to implement a selfie photo verification flow using a Nigeria BVN. There are three steps involved.


### 1. Verify the BVN

The first step is to verify the BVN. An OTP will be sent to the phone number of the BVN owner to confirm.

Example request:
```
curl --location --request POST 'https://api.appruve.co/v1/verifications/bvn/verify' \
--header 'Authorization: Bearer <Token>' \
--header 'Content-Type: application/json' \
--data-raw '{
	"bvn": "22207809091"
}'
```

Example response:
```
{
    "success": true,
    "transaction_reference": "93ca2d74-e11e-4044-82d9-da7db4225045"
}
```

### 2. Validate the OTP

Verify the OTP confirmed by the BVN owner.

Example request:
```
curl --location --request POST 'https://api.appruve.co/v1/verifications/bvn/validate' \
--header 'Authorization: Bearer <Token>' \
--header 'Content-Type: application/json' \
--data-raw '{
	"transaction_reference": "93ca2d74-e11e-4044-82d9-da7db4225045",
	"otp": "186522"
}'
```

Example response:
```
{
    "success": true,
    "message": "BVN successfully verified!"
}
```

### 3. Upload the selfie photo

Upload the selfie photo taken by the user. Appruve will match the face on the selfie photo against the face on the matched BVN record.

Example request:
```
curl --location --request POST 'https://api.appruve.co/v1/verifications/selfie_image/upload' \
--header 'Authorization: Bearer <Token>' \
--form 'selfie_image=@/Users/img.jpg' \
--form 'transaction_reference=93ca2d74-e11e-4044-82d9-da7db4225045'
```

Example response:
```
{
  "success": true,
  "id": "93ca2d74-e11e-4044-82d9-da7db4225045",
  "confidence_score": 100,
  "id_type": "bvn",
  "country_code": "ng",
  "status": "success",
  "face_on_id_document": "url",
  "face_match_percent": 100,
  "id_match_percent": 100,
  "selfie_face_url": "url",
  "selfie_image_submitted": "url",
  "id_face_sharpness": 87,
  "id_face_brightness": 96,
  "selfie_face_sharpness": 88,
  "selfie_face_brightness": 90,
  "id_details": {},
  "created_at": "2020-05-10",
  "custom_params": {}
}
```