Task 1. Create a bucket

export BUCKET=$(gcloud config get-value project)

gsutil mb "gs://$BUCKET"

Task 2. Create a retention policy

gsutil retention set 30s "gs://$BUCKET"

gsutil retention get "gs://$BUCKET"

Task 3. Add a file to the bucket

curl https://upload.wikimedia.org/wikipedia/commons/thumb/a/a4/Ada_Lovelace_portrait.jpg/800px-Ada_Lovelace_portrait.jpg --output ada.jpg

gsutil cp ada.jpg gs://$BUCKET

gsutil acl ch -u AllUsers:R gs://$BUCKET/ada.jpg


