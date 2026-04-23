# 就活スケジュール管理アプリ

## 概要
就活のタスク（面接・適性検査など）を管理するシンプルなアプリです。

## 機能
- タスク追加
- 締切設定
- 残り時間表示
- 削除機能

## 使用技術
- HTML
- CSS
- JavaScript

## 動作方法
index.html をブラウザで開くことで動作します。

## 動作イメージ
![アプリ画面](screenshot.png)

## 工夫した点
締切までの残り時間を自動で計算して表示するようにしました。

## 改善点
データの保存機能（localStorage）を今後追加予定です。

### ■ 何を作ろうと思ったか
就職活動における面接や適性検査などのスケジュールを管理できるシンプルなWebアプリを作成しました。

### ■ なぜ作ろうと思ったか
就職活動では複数の企業の選考が並行して進むため、スケジュール管理が煩雑になりやすいと感じました。  
そのため、締切や予定を簡単に管理できるツールがあれば便利だと考え、このアプリを作成しました。

### ■ 生成AIとのやり取り
今回の制作では、生成AIを活用して基本的な構造の整理やコードの改善を行いました。

特に以下の点で活用しました：
- JavaScriptでのタスク追加・削除機能の実装方法
- 残り時間の計算ロジックの考え方
- コードのシンプル化や可読性の向上

また、単にコードを生成するだけでなく、
「なぜその実装になるのか」を意識して質問することで理解を深めるよう工夫しました。

### ■ 詰まったところとどう乗り越えたか
タスクの締切から残り時間を計算する処理の実装に苦戦しました。  
特に、日付データの扱いと現在時刻との差分の計算で混乱しました。

この問題については、生成AIに質問しながら処理の流れを分解して理解することで解決しました。  
また、実際にコンソールで値を確認しながら動作を検証することで理解を深めました。

### ■ 次にやるなら何を変えるか
今回のアプリはページを更新するとデータが消えてしまうため、  
次回は localStorage などを活用してデータを保存できるように改善したいと考えています。

また、UIの見やすさや操作性も向上させ、より実用的なアプリにしたいです。

コード
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<title>就活スケジュール管理</title>
<style>
body {
  font-family: Arial;
  max-width: 600px;
  margin: auto;
}
input, button {
  margin: 5px;
}
.task {
  border: 1px solid #ccc;
  padding: 10px;
  margin-top: 5px;
}
</style>
</head>
<body>

<h2>就活スケジュール管理</h2>

<input type="text" id="taskInput" placeholder="内容（例：面接・適性検査）">
<input type="datetime-local" id="deadline">
<button onclick="addTask()">追加</button>

<div id="taskList"></div>

<script>
let tasks = [];

function addTask() {
  const taskInput = document.getElementById("taskInput").value;
  const deadline = document.getElementById("deadline").value;

  if (!taskInput || !deadline) {
    alert("入力してください");
    return;
  }

  tasks.push({ taskInput, deadline });
  renderTasks();
}

function renderTasks() {
  const list = document.getElementById("taskList");
  list.innerHTML = "";

  tasks.forEach((task, index) => {
    const now = new Date();
    const end = new Date(task.deadline);
    const diff = Math.floor((end - now) / (1000 * 60 * 60));

    const div = document.createElement("div");
    div.className = "task";
    div.innerHTML = `
      <strong>${task.taskInput}</strong><br>
      締切：${task.deadline}<br>
      残り：約 ${diff} 時間<br>
      <button onclick="deleteTask(${index})">削除</button>
    `;
    list.appendChild(div);
  });
}

function deleteTask(index) {
  tasks.splice(index, 1);
  renderTasks();
}
</script>

</body>
</html>
