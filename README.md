# BeMyValentine
import requests

# Replace with your GitHub credentials
GITHUB_USERNAME = "YOUR_GITHUB_USERNAME"
GITHUB_TOKEN = "YOUR_GITHUB_TOKEN"
REPO_NAME = "BeMyValentine"

# API URLs
REPO_URL = f"https://api.github.com/user/repos"
ISSUE_URL = f"https://api.github.com/repos/{GITHUB_USERNAME}/{REPO_NAME}/issues"

# Valentine message for README
README_CONTENT = """# 💖 Will You Be My Valentine? 💖

Hey Barnabas!

I was trying to find the best way to ask you… so I forked my heart into this repo.  
Would you accept my pull request to your heart? 😍

If YES, please:  
✅ Star this repo  
✅ Create a new issue titled "Yes!"  

If NO… well, I’ll just handle the merge conflict 😅  

Happy Valentine's Day! ❤️
"""

# Create a new GitHub repository
repo_data = {
    "name": REPO_NAME,
    "description": "A special Valentine's Day repo 💕",
    "private": False
}

response = requests.post(REPO_URL, json=repo_data, auth=(GITHUB_USERNAME, GITHUB_TOKEN))

if response.status_code == 201:
    print(f"✅ Repository '{REPO_NAME}' created successfully!")
else:
    print(f"❌ Failed to create repo: {response.json()}")

# Add README file using GitHub API
FILE_URL = f"https://api.github.com/repos/{GITHUB_USERNAME}/{REPO_NAME}/contents/README.md"

file_data = {
    "message": "Add Valentine's message",
    "content": README_CONTENT.encode('utf-8').hex(),  # Encoding content
    "committer": {
        "name": GITHUB_USERNAME,
        "email": f"{GITHUB_USERNAME}@users.noreply.github.com"
    }
}

response = requests.put(FILE_URL, json=file_data, auth=(GITHUB_USERNAME, GITHUB_TOKEN))

if response.status_code == 201:
    print("✅ README.md added successfully!")
else:
    print(f"❌ Failed to add README: {response.json()}")

# Open a GitHub issue
issue_data = {
    "title": "Will You Be My Valentine? 💌",
    "body": "Hey there! I've created this special repo just for you. Check it out and let me know what you think! 💖"
}

response = requests.post(ISSUE_URL, json=issue_data, auth=(GITHUB_USERNAME, GITHUB_TOKEN))
if response.status_code == 201:
    print("✅ Issue created successfully!")
else:
    print(f"❌ Failed to create issue: {response.json()}")

print(f"\n🎉 Done! Share this link: https://github.com/{GITHUB_USERNAME}/{REPO_NAME}")
