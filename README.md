#!/bin/bash

set -e  # Exit immediately if a command exits with a non-zero status

# Prompt for the repository name
read -p "Enter your repository name: " repo_name

# Create a new directory for the repository
mkdir -p /workspaces/ton-onboarding-challenge/$repo_name
cd /workspaces/ton-onboarding-challenge/$repo_name

# Initialize a new git repository
git init

# Create a .gitconfig file if it doesn't exist
if [ ! -f "/workspaces/ton-onboarding-challenge/.gitconfig" ]; then
    echo "Creating .gitconfig file..."
    touch /workspaces/ton-onboarding-challenge/.gitconfig
fi

# Create a README.md file
echo "# $repo_name" > README.md

# Add files to the repository and make an initial commit
git add .
git commit -m "Initial commit with README.md"

# Enable git remote by setting the remote URL
if [ -n "$itrepository" ]; then
    remote_url=$itrepository
else
    read -p "Enter your remote repository URL: " remote_url
fi

if [ -z "$remote_url" ]; then
    echo "Remote URL cannot be empty. Please enter a valid remote repository URL."
    exit 1
fi

if [[ $remote_url =~ ^https?:// ]]; then
    git remote add origin $remote_url
    git push -u origin master
else
    echo "Invalid URL. Please enter a valid remote repository URL."
fi
