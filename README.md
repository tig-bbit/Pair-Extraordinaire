# Pair-Extraordinaire
How to get the Golden Pair Extraordinaire badge?

#!/bin/bash

# Define co-authors' names and emails
COAUTHOR_1_NAME="rabiger"
COAUTHOR_1_EMAIL="krisato93@outlook.com"

COAUTHOR_2_NAME="Jane Smith"
COAUTHOR_2_EMAIL="jane.smith@example.com"

COAUTHOR_3_NAME="Alice Johnson"
COAUTHOR_3_EMAIL="alice.johnson@example.com"

# Loop to create and merge pull requests 10 times
for i in {1..10}
do
    # Make a change in the dev branch
    echo "NEW_ENV_VARIABLE='value'" >> .envexample

    # Step 2: Add changes to git
    git add .envexample

    # Commit with multiple co-authors using a here document
    git commit -F - <<EOF
Update .envexample for change #$i.

Co-authored-by: $COAUTHOR_1_NAME <$COAUTHOR_1_EMAIL>
EOF

    # Push changes to the dev branch
    git push origin dev

    # Create a pull request from dev to main using GitHub CLI
    pr_number=$(gh pr create --base main --head dev --title "Merge dev to main for change #$i" --body "Merging changes from dev to main for change #$i.")
    
    echo "Created pull request #$pr_number"

    # Merge the pull request
    gh pr merge $pr_number --merge
    echo "Merged pull request #$pr_number"

    # Optional: Wait for a short period to ensure timing
    sleep_duration=$((RANDOM % 3 + 1))  # Random sleep between 1 and 3 seconds
    echo "Sleeping for $sleep_duration seconds..."
    sleep $sleep_duration  # Sleep for the random duration
done
