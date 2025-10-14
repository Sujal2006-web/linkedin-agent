import requests
import openai

# Configuration
GITHUB_REPO_OWNER = 'your-github-username'  # Replace with your GitHub username
GITHUB_REPO_NAME = 'your-repo-name'  # Replace with your repository name
GITHUB_TOKEN = 'YOUR_GITHUB_TOKEN'  # Your GitHub Personal Access Token
OPENAI_API_KEY = 'YOUR_OPENAI_API_KEY'  # Your OpenAI API Key

# Step 1: Fetch data from GitHub
def fetch_github_repo_description(owner, repo, token):
    headers = {
        'Authorization': f'token {token}',
        'Accept': 'application/vnd.github.v3+json'
    }
    url = f'https://api.github.com/repos/{owner}/{repo}'
    response = requests.get(url, headers=headers)
    
    if response.status_code == 200:
        data = response.json()
        return data.get('description', 'No description available.')
    else:
        raise Exception(f"GitHub API Error: {response.status_code} - {response.text}")

# Step 2: Use OpenAI to generate LinkedIn post content
def generate_linkedin_post(description):
    openai.api_key = OPENAI_API_KEY
    
    prompt = f"Based on this GitHub repository description: '{description}', create a professional LinkedIn post that highlights the project's key features and invites connections to collaborate. Keep it under 200 words."
    
    response = openai.Completion.create(
        engine="text-davinci-003",  # You can use newer models like gpt-3.5-turbo if available
        prompt=prompt,
        max_tokens=200,
        n=1,
        stop=None,
        temperature=0.7,
    )
    
    return response.choices[0].text.strip()

# Step 3: Main function to run the agent
def run_linkedin_ai_agent():
    try:
        repo_description = fetch_github_repo_description(GITHUB_REPO_OWNER, GITHUB_REPO_NAME, GITHUB_TOKEN)
        linkedin_post = generate_linkedin_post(repo_description)
        
        print("Generated LinkedIn Post:")
        print(linkedin_post)
        
        # In a full implementation, you could use this output to post to LinkedIn via their API or a tool like n8n.
    except Exception as e:
        print(f"Error: {e}")

# Run the script
if __name__ == "__main__":
    run_linkedin_ai_agent()
