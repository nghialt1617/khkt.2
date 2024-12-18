#!/bin/bash

# Cấu hình
USERNAME="your-username"       # Thay 'your-username' bằng tên GitHub của bạn
REPO_NAME="climate-prediction" # Tên repository bạn muốn tạo
DESCRIPTION="Climate prediction web application"
BRANCH="main"

# Tạo file HTML
echo "Tạo file index.html..."
cat > index.html <<EOF
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dự đoán khí hậu</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            color: #ffffff;
            background: linear-gradient(135deg, #1e90ff, #87cefa);
            background-attachment: fixed;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        h1, h2 {
            text-align: center;
        }
        form {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
            width: 80%;
            max-width: 600px;
        }
        label {
            font-size: 1.1em;
            margin-top: 10px;
            display: inline-block;
        }
        input, button {
            width: calc(100% - 20px);
            padding: 10px;
            margin-top: 10px;
            border: none;
            border-radius: 5px;
        }
        button {
            background-color: #32cd32;
            color: white;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #228b22;
        }
        #result {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            padding: 15px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Dự đoán khí hậu</h1>
    <form>
        <label for="currentTemp">Nhiệt độ hiện tại (°C):</label>
        <input type="number" id="currentTemp" value="15" required>
        <button type="button" onclick="alert('Chức năng chưa được triển khai!')">Dự đoán</button>
    </form>
    <p id="result">Kết quả dự đoán sẽ hiển thị ở đây.</p>
</body>
</html>
EOF

# Khởi tạo Git và commit
echo "Khởi tạo Git repository..."
git init
git add index.html
git commit -m "Initial commit - Climate Prediction Web App"

# Tạo repository trên GitHub bằng GitHub CLI
echo "Tạo repository trên GitHub..."
gh repo create "$REPO_NAME" --public --description "$DESCRIPTION" --confirm

# Kết nối remote và đẩy code lên GitHub
git remote add origin "https://github.com/$USERNAME/$REPO_NAME.git"
git branch -M $BRANCH
git push -u origin $BRANCH

# Triển khai GitHub Pages
echo "Triển khai GitHub Pages..."gh api repos/$USERNAME/$REPO_NAME/pages --method POST --field source.branch=$BRANCH --field source.path=/

echo "Deployment thành công! Truy cập trang web tại:"
echo "https://$USERNAME.github.io/$REPO_NAME"
