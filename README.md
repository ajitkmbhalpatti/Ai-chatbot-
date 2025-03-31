<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Ai asistant</title>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>    
    :root {
            /* Light Theme Colors */
            --primary-light: #0088cc;
            --primary-dark: #006699;
            --user-message-light: #e3f2fd;
            --bot-message-light: #f1f1f1;
            --system-message-light: #e3f2fd;
            --text-light: #333333;
            --text-secondary-light: #666666;
            --background-light: #f5f7fa;
            --card-light: #ffffff;
            --success-light: #4caf50;
            --error-light: #f44336;
            --warning-light: #ff9800;
            --typing-light: #e0e0e0;

            /* Dark Theme Colors */
            --primary-darkmode: #5db0e6;
            --user-message-dark: #1e3a5f;
            --bot-message-dark: #2d2d2d;
            --system-message-dark: #3a3a3a;
            --text-dark: #e1e1e1;
            --text-secondary-dark: #aaaaaa;
            --background-dark: #121212;
            --card-dark: #1e1e1e;
            --success-dark: #66bb6a;
            --error-dark: #f44336;
            --warning-dark: #ffa726;
            --typing-dark: #333333;

            /* Current Theme Variables */
            --primary: var(--primary-light);
            --user-message: var(--user-message-light);
            --bot-message: var(--bot-message-light);
            --system-message: var(--system-message-light);
            --text: var(--text-light);
            --text-secondary: var(--text-secondary-light);
            --background: var(--background-light);
            --card: var(--card-light);
            --success: var(--success-light);
            --error: var(--error-light);
            --warning: var(--warning-light);
            --typing: var(--typing-light);

            /* Other Variables */
            --border-radius: 12px;
            --border-radius-sm: 8px;
            --border-radius-circle: 50%;
            --shadow-sm: 0 1px 3px rgba(0,0,0,0.12);
            --shadow-md: 0 4px 6px rgba(0,0,0,0.1);
            --shadow-lg: 0 10px 15px rgba(0,0,0,0.1);
            --spacing-xs: 4px;
            --spacing-sm: 8px;
            --spacing-md: 16px;
            --spacing-lg: 24px;
            --spacing-xl: 32px;
            --transition: 0.3s ease;
        }

        .dark-mode {
            --primary: var(--primary-darkmode);
            --user-message: var(--user-message-dark);
            --bot-message: var(--bot-message-dark);
            --system-message: var(--system-message-dark);
            --text: var(--text-dark);
            --text-secondary: var(--text-secondary-dark);
            --background: var(--background-dark);
            --card: var(--card-dark);
            --success: var(--success-dark);
            --error: var(--error-dark);
            --warning: var(--warning-dark);
            --typing: var(--typing-dark);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--background);
            color: var(--text);
            line-height: 1.6;
            overscroll-behavior-y: contain;
            transition: background-color var(--transition), color var(--transition);
        }

        .container {
            max-width: 100%;
            margin: 0 auto;
            padding: 0;
            height: 100vh;
            display: flex;
            flex-direction: column;
            position: relative;
            overflow: hidden;
        }

        /* Header */
        .header {
            padding: var(--spacing-md);
            background-color: var(--primary);
            color: white;
            display: flex;
            align-items: center;
            justify-content: space-between;
            box-shadow: var(--shadow-sm);
            z-index: 10;
            position: relative;
        }

        .header-content {
            display: flex;
            align-items: center;
            gap: var(--spacing-md);
            width: 100%;
        }

        .chat-info {
            display: flex;
            align-items: center;
            gap: var(--spacing-sm);
            flex: 1;
        }

        .chat-avatar {
            width: 40px;
            height: 40px;
            border-radius: var(--border-radius-circle);
            background-color: rgba(255,255,255,0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 600;
            color: white;
        }

        .chat-details {
            display: flex;
            flex-direction: column;
        }

        .chat-title {
            font-weight: 600;
            font-size: 1.1rem;
        }

        .chat-status {
            font-size: 0.8rem;
            display: flex;
            align-items: center;
            gap: var(--spacing-xs);
        }

        .status-indicator {
            width: 8px;
            height: 8px;
            border-radius: var(--border-radius-circle);
        }

        .online {
            background-color: var(--success);
            animation: pulse 1.5s infinite;
        }

        .offline {
            background-color: var(--error);
        }

        .typing-status {
            background-color: var(--warning);
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { opacity: 1; }
            50% { opacity: 0.5; }
            100% { opacity: 1; }
        }

        /* Dark Mode Toggle */
        .theme-toggle {
            background: none;
            border: none;
            color: white;
            font-size: 1.2rem;
            width: 36px;
            height: 36px;
            border-radius: var(--border-radius-circle);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: background-color var(--transition);
        }

        .theme-toggle:hover {
            background-color: rgba(255,255,255,0.1);
        }

        /* Messages Container */
        .messages {
            flex: 1;
            padding: var(--spacing-md);
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            background-color: var(--background);
            scroll-behavior: smooth;
        }

        /* Message Styling */
        .message {
            max-width: 85%;
            margin-bottom: var(--spacing-md);
            padding: var(--spacing-sm) var(--spacing-md);
            border-radius: var(--border-radius);
            word-wrap: break-word;
            position: relative;
            animation: fadeIn 0.3s ease-out;
            box-shadow: var(--shadow-sm);
            transition: all var(--transition);
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .incoming {
            align-self: flex-start;
            background-color: var(--bot-message);
            border-bottom-left-radius: var(--border-radius-sm);
        }

        .outgoing {
            align-self: flex-end;
            background-color: var(--user-message);
            border-bottom-right-radius: var(--border-radius-sm);
        }

        .system {
            align-self: center;
            background-color: var(--system-message);
            text-align: center;
            max-width: 90%;
            padding: var(--spacing-xs) var(--spacing-md);
            border-radius: 20px;
            font-size: 0.85rem;
            color: var(--text-secondary);
        }

        .message:hover {
            transform: translateY(-2px);
            box-shadow: var(--shadow-md);
        }

        .message-header {
            display: flex;
            justify-content: space-between;
            margin-bottom: var(--spacing-xs);
        }

        .sender-name {
            font-weight: 600;
            font-size: 0.85rem;
            color: var(--primary);
        }

        .message-time {
            font-size: 0.75rem;
            color: var(--text-secondary);
            display: flex;
            align-items: center;
            gap: var(--spacing-xs);
        }

        .message-content {
            font-size: 0.95rem;
            line-height: 1.5;
        }

        /* Message Actions */
        .message-actions {
            position: absolute;
            top: -12px;
            right: -8px;
            display: flex;
            gap: var(--spacing-xs);
            opacity: 0;
            transform: scale(0.9);
            transition: all var(--transition);
        }

        .message:hover .message-actions {
            opacity: 1;
            transform: scale(1);
        }

        .action-btn {
            background-color: var(--card);
            color: var(--text-secondary);
            border: none;
            border-radius: var(--border-radius-circle);
            width: 28px;
            height: 28px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: var(--shadow-sm);
            transition: all var(--transition);
        }

        .action-btn:hover {
            transform: scale(1.1);
        }

        .reply-btn:hover {
            color: var(--primary);
        }

        .edit-btn:hover {
            color: var(--warning);
        }

        .delete-btn:hover {
            color: var(--error);
        }

        /* Typing Indicator */
        .typing-indicator {
            align-self: flex-start;
            background-color: var(--typing);
            padding: var(--spacing-sm) var(--spacing-md);
            border-radius: var(--border-radius);
            margin-bottom: var(--spacing-md);
            display: none;
            box-shadow: var(--shadow-sm);
        }

        .typing-content {
            display: flex;
            align-items: center;
            gap: var(--spacing-sm);
        }

        .typing-text {
            font-size: 0.85rem;
            color: var(--text-secondary);
        }

        .typing-dots {
            display: flex;
            gap: 4px;
        }

        .typing-dot {
            width: 8px;
            height: 8px;
            border-radius: var(--border-radius-circle);
            background-color: var(--text-secondary);
            animation: typingAnimation 1.4s infinite ease-in-out;
        }

        .typing-dot:nth-child(1) {
            animation-delay: 0s;
        }

        .typing-dot:nth-child(2) {
            animation-delay: 0.2s;
        }

        .typing-dot:nth-child(3) {
            animation-delay: 0.4s;
        }

        @keyframes typingAnimation {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-4px); }
        }

        /* Input Area */
        .input-area {
            padding: var(--spacing-md);
            background-color: var(--card);
            border-top: 1px solid rgba(0, 0, 0, 0.1);
            display: flex;
            align-items: center;
            gap: var(--spacing-sm);
            z-index: 5;
        }

        #message-input {
            flex: 1;
            padding: var(--spacing-sm) var(--spacing-md);
            border: 1px solid rgba(0, 0, 0, 0.1);
            border-radius: var(--border-radius);
            outline: none;
            font-size: 0.95rem;
            transition: all var(--transition);
            background-color: var(--card);
            color: var(--text);
            min-height: 40px;
            max-height: 120px;
            resize: none;
            line-height: 1.5;
        }

        #message-input:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 2px rgba(0, 136, 204, 0.2);
        }

        #send-btn {
            background-color: var(--primary);
            color: white;
            border: none;
            border-radius: var(--border-radius-circle);
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 1.2rem;
            transition: all var(--transition);
        }

        #send-btn:disabled {
            background-color: var(--text-secondary);
            cursor: not-allowed;
            transform: none;
        }

        #send-btn:hover:not(:disabled) {
            background-color: var(--primary-dark);
            transform: scale(1.05);
        }

        /* Status Bar */
        .status-bar {
            padding: var(--spacing-sm) var(--spacing-md);
            background-color: var(--card);
            border-top: 1px solid rgba(0, 0, 0, 0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 0.8rem;
            color: var(--text-secondary);
        }

        .connection-status {
            display: flex;
            align-items: center;
            gap: var(--spacing-xs);
        }

        .last-updated {
            display: flex;
            align-items: center;
            gap: var(--spacing-xs);
        }

        /* Context Menu */
        .context-menu {
            position: fixed;
            background-color: var(--card);
            border-radius: var(--border-radius-sm);
            box-shadow: var(--shadow-lg);
            z-index: 30;
            display: none;
            flex-direction: column;
            min-width: 180px;
            overflow: hidden;
        }

        .context-item {
            padding: var(--spacing-sm) var(--spacing-md);
            display: flex;
            align-items: center;
            gap: var(--spacing-md);
            cursor: pointer;
            transition: background-color var(--transition);
        }

        .context-item:hover {
            background-color: rgba(0, 0, 0, 0.05);
        }

        .context-item i {
            width: 20px;
            color: var(--text-secondary);
        }

        .context-item.delete {
            color: var(--error);
        }

        .context-item.delete i {
            color: var(--error);
        }

        /* Reply Preview */
        .reply-preview {
            padding: var(--spacing-sm) var(--spacing-md);
            background-color: rgba(0, 0, 0, 0.05);
            border-left: 3px solid var(--primary);
            border-radius: var(--border-radius-sm);
            margin-bottom: var(--spacing-sm);
            display: none;
        }

        .reply-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: var(--spacing-xs);
        }

        .reply-title {
            font-size: 0.8rem;
            font-weight: 600;
            color: var(--primary);
        }

        .cancel-reply {
            background: none;
            border: none;
            color: var(--text-tertiary);
            font-size: 0.9rem;
            cursor: pointer;
        }

        .reply-content {
            font-size: 0.85rem;
            color: var(--text-secondary);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        /* Loading spinner */
        .spinner {
            width: 24px;
            height: 24px;
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s ease-in-out infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }

        ::-webkit-scrollbar-track {
            background: rgba(0, 0, 0, 0.05);
        }

        ::-webkit-scrollbar-thumb {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 3px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: rgba(0, 0, 0, 0.3);
        }

        /* Selection color */
        ::selection {
            background-color: var(--primary);
            color: white;
        }

        /* Media queries for responsive design */
        @media (max-width: 768px) {
            .message {
                max-width: 90%;
            }
        }

        @media (max-width: 480px) {
            :root {
                --spacing-md: 12px;
                --spacing-lg: 16px;
            }
            
            #message-input {
                font-size: 0.9rem;
            }
            
            .message-content {
                font-size: 0.9rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <div class="header">
            <div class="header-content">
                <div class="chat-info">
                    <div class="chat-avatar">T</div>
                    <div class="chat-details">
                        <div class="chat-title">Study ai</div>
                        <div class="chat-status">
                            <div class="status-indicator offline" id="status-indicator"></div>
                            <span id="status-text">Connecting...</span>
                        </div>
                    </div>
                </div>
                <button class="theme-toggle" id="theme-toggle">
                    <i class="fas fa-moon"></i>
                </button>
            </div>
        </div>

        <!-- Messages Container -->
        <div class="messages" id="messages">
            <!-- Messages will appear here -->
        </div>

        <!-- Reply Preview -->
        <div class="reply-preview" id="reply-preview">
            <div class="reply-header">
                <div class="reply-title">Replying to message</div>
                <button class="cancel-reply" id="cancel-reply">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <div class="reply-content" id="reply-content"></div>
        </div>

        <!-- Input Area -->
        <div class="input-area">
            <textarea id="message-input" placeholder="Type your message..." rows="1" autocomplete="off"></textarea>
            <button id="send-btn">
                <i class="fas fa-paper-plane"></i>
            </button>
        </div>

        <!-- Status Bar -->
        <div class="status-bar">
            <div class="connection-status">
                <div class="status-indicator offline" id="connection-status"></div>
                <span id="connection-text">Connecting...</span>
            </div>
            <div class="last-updated">
                <i class="far fa-clock"></i>
                <span id="last-updated"></span>
            </div>
        </div>

        <!-- Context Menu -->
        <div class="context-menu" id="context-menu">
            <div class="context-item reply">
                <i class="fas fa-reply"></i>
                <span>Reply</span>
            </div>
            <div class="context-item edit">
                <i class="fas fa-edit"></i>
                <span>Edit</span>
            </div>
            <div class="context-item delete">
                <i class="fas fa-trash-alt"></i>
                <span>Delete</span>
            </div>
        </div>
    </div>

    <script>
      
 // Telegram bot configuration
        const TELEGRAM_BOT_TOKEN = "8171736099:AAEApO-XiDvFN0_AzsYG782VNzGTbwUN9pQ";
        
        // DOM elements
        const messagesContainer = document.getElementById('messages');
        const messageInput = document.getElementById('message-input');
        const sendBtn = document.getElementById('send-btn');
        const statusIndicator = document.getElementById('status-indicator');
        const statusText = document.getElementById('status-text');
        const connectionStatus = document.getElementById('connection-status');
        const connectionText = document.getElementById('connection-text');
        const lastUpdated = document.getElementById('last-updated');
        const themeToggle = document.getElementById('theme-toggle');
        const contextMenu = document.getElementById('context-menu');
        const replyPreview = document.getElementById('reply-preview');
        const replyContent = document.getElementById('reply-content');
        const cancelReply = document.getElementById('cancel-reply');
        
        // State variables
        let lastUpdateId = 0;
        let isConnected = false;
        let currentChatId = null;
        let replyingToMessageId = null;
        let editingMessageId = null;
        let isTyping = false;
        let lastTypingTime = 0;
        const messageMap = new Map(); // Stores message_id to DOM element mapping
        let contextMenuMessageId = null;

        // Initialize connection
        initConnection();

        // Auto-resize textarea
        messageInput.addEventListener('input', function() {
            this.style.height = 'auto';
            this.style.height = (this.scrollHeight) + 'px';
            
            // Show typing indicator if user is typing
            if (!isTyping && this.value.trim().length > 0) {
                isTyping = true;
                updateTypingStatus(true);
            }
            
            lastTypingTime = Date.now();
        });

        // Check if user stopped typing
        setInterval(() => {
            if (isTyping && Date.now() - lastTypingTime > 2000) {
                isTyping = false;
                updateTypingStatus(false);
            }
        }, 1000);

        // Theme toggle
        themeToggle.addEventListener('click', () => {
            document.body.classList.toggle('dark-mode');
            const icon = themeToggle.querySelector('i');
            if (document.body.classList.contains('dark-mode')) {
                icon.classList.remove('fa-moon');
                icon.classList.add('fa-sun');
                localStorage.setItem('theme', 'dark');
            } else {
                icon.classList.remove('fa-sun');
                icon.classList.add('fa-moon');
                localStorage.setItem('theme', 'light');
            }
        });

        // Check for saved theme preference
        if (localStorage.getItem('theme') === 'dark') {
            document.body.classList.add('dark-mode');
            themeToggle.querySelector('i').classList.remove('fa-moon');
            themeToggle.querySelector('i').classList.add('fa-sun');
        }

        // Cancel reply
        cancelReply.addEventListener('click', () => {
            replyingToMessageId = null;
            replyPreview.style.display = 'none';
        });

        // Close context menu when clicking elsewhere
        document.addEventListener('click', (e) => {
            if (!contextMenu.contains(e.target)) {
                contextMenu.style.display = 'none';
            }
        });

        // Handle context menu actions
        document.querySelectorAll('.context-item').forEach(item => {
            item.addEventListener('click', () => {
                if (!contextMenuMessageId) return;
                
                const messageData = messageMap.get(contextMenuMessageId);
                if (!messageData) return;
                
                contextMenu.style.display = 'none';
                
                if (item.classList.contains('reply')) {
                    replyingToMessageId = contextMenuMessageId;
                    editingMessageId = null;
                    replyContent.textContent = messageData.element.querySelector('.message-content').textContent;
                    replyPreview.style.display = 'block';
                    messageInput.focus();
                } 
                else if (item.classList.contains('edit')) {
                    // Only allow editing outgoing messages
                    if (messageData.isOutgoing) {
                        editingMessageId = contextMenuMessageId;
                        replyingToMessageId = null;
                        messageInput.value = messageData.element.querySelector('.message-content').textContent;
                        messageInput.focus();
                        replyPreview.style.display = 'none';
                    } else {
                        addSystemMessage("You can only edit your own messages");
                    }
                }
                else if (item.classList.contains('delete')) {
                    deleteMessage(contextMenuMessageId, messageData.chatId);
                }
            });
        });

        async function initConnection() {
            try {
                updateConnectionStatus(false, "Connecting to Telegram...");
                
                // Check if bot token is valid
                const meUrl = `https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/getMe`;
                const meResponse = await fetch(meUrl);
                const meData = await meResponse.json();
                
                if (!meData.ok) {
                    updateConnectionStatus(false, "Invalid Telegram Bot Token");
                    return;
                }
                
                updateConnectionStatus(true, "Connected to study ai");
                
                // Start polling for updates
                pollTelegramUpdates();
                
            } catch (error) {
                updateConnectionStatus(false, "Connection failed");
                addSystemMessage("Connection error: " + error.message);
                setTimeout(initConnection, 5000); // Retry after 5 seconds
            }
        }

        async function pollTelegramUpdates() {
            try {
                const updatesUrl = `https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/getUpdates?offset=${lastUpdateId + 1}&timeout=60`;
                const response = await fetch(updatesUrl);
                const data = await response.json();
                
                if (data.ok) {
                    if (data.result.length > 0) {
                        lastUpdateId = data.result[data.result.length - 1].update_id;
                        updateLastUpdated();
                        
                        // Process each new message
                        for (const update of data.result) {
                            if (update.message && !update.message.from.is_bot) {
                                currentChatId = update.message.chat.id;
                                await processIncomingMessage(update.message);
                            }
                        }
                    }
                    
                    // Continue polling
                    setTimeout(pollTelegramUpdates, 1000);
                } else {
                    updateConnectionStatus(false, "Telegram API error");
                    setTimeout(pollTelegramUpdates, 5000); // Retry after 5 seconds
                }
            } catch (error) {
                console.error("Polling error:", error);
                updateConnectionStatus(false, "Connection lost - reconnecting...");
                setTimeout(pollTelegramUpdates, 5000); // Retry after 5 seconds
            }
        }

        async function processIncomingMessage(message) {
            // Show typing indicator
            showTypingIndicator(message.from.first_name);
            
            // Simulate typing delay
            await new Promise(resolve => setTimeout(resolve, 1000 + Math.random() * 2000));
            
            // Hide typing indicator
            hideTypingIndicator();
            
            // Display the received message
            const messageElement = addIncomingMessage(
                message.from.first_name, 
                message.text,
                message.message_id,
                new Date(message.date * 1000)
            );
            
            // Store in message map for future actions
            messageMap.set(message.message_id, {
                element: messageElement,
                chatId: message.chat.id,
                isOutgoing: false
            });
            
            // Add context menu handler
            messageElement.addEventListener('contextmenu', (e) => {
                e.preventDefault();
                showContextMenu(e, message.message_id);
            });
            
            // Add click handler for mobile
            messageElement.addEventListener('click', () => {
                showMessageActions(messageElement, message.message_id);
            });
        }

        async function sendMessageToTelegram(text) {
            if (!currentChatId) {
                addSystemMessage("Error: No active chat to reply to");
                return;
            }
            
            try {
                // Disable send button while sending
                sendBtn.disabled = true;
                sendBtn.innerHTML = '<div class="spinner"></div>';
                
                let params = {
                    chat_id: currentChatId,
                    text: text
                };
                
                // Add reply parameters if replying
                if (replyingToMessageId) {
                    params.reply_to_message_id = replyingToMessageId;
                }
                
                // Display the outgoing message (temporarily without message_id)
                const tempMessageElement = addOutgoingMessage(
                    "You", 
                    text, 
                    null,
                    new Date()
                );
                
                // If editing, update the existing message
                if (editingMessageId) {
                    const response = await fetch(`https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/editMessageText`, {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify({
                            chat_id: currentChatId,
                            message_id: editingMessageId,
                            text: text
                        })
                    });
                    
                    const responseData = await response.json();
                    
                    if (responseData.ok) {
                        // Update the existing message in UI
                        const existingMessageData = messageMap.get(editingMessageId);
                        if (existingMessageData) {
                            existingMessageData.element.querySelector('.message-content').textContent = text;
                            existingMessageData.element.querySelector('.message-time').innerHTML = `
                                ${formatTime(new Date())} <i class="fas fa-check-double" style="color: var(--success);"></i>
                            `;
                        }
                        
                        // Remove the temporary message
                        tempMessageElement.remove();
                    } else {
                        throw new Error(responseData.description || "Failed to edit message");
                    }
                } 
                // Otherwise send new message
                else {
                    const response = await fetch(`https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage`, {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify(params)
                    });
                    
                    const responseData = await response.json();
                    
                    if (responseData.ok) {
                        // Update the message with the actual message_id from Telegram
                        const messageId = responseData.result.message_id;
                        tempMessageElement.dataset.messageId = messageId;
                        
                        // Update the timestamp with server time
                        tempMessageElement.querySelector('.message-time').innerHTML = `
                            ${formatTime(new Date(responseData.result.date * 1000))} <i class="fas fa-check-double"></i>
                        `;
                        
                        // Store in message map for future actions
                        messageMap.set(messageId, {
                            element: tempMessageElement,
                            chatId: currentChatId,
                            isOutgoing: true
                        });
                        
                        // Add context menu handler
                        tempMessageElement.addEventListener('contextmenu', (e) => {
                            e.preventDefault();
                            showContextMenu(e, messageId);
                        });
                        
                        // Add click handler for mobile
                        tempMessageElement.addEventListener('click', () => {
                            showMessageActions(tempMessageElement, messageId);
                        });
                    } else {
                        throw new Error(responseData.description || "Failed to send message");
                    }
                }
                
                // Clear input and reset state
                messageInput.value = '';
                resetInputState();
                
            } catch (error) {
                console.error("Error sending message:", error);
                addSystemMessage("Failed to send message: " + error.message);
            } finally {
                // Re-enable send button
                sendBtn.disabled = false;
                sendBtn.innerHTML = '<i class="fas fa-paper-plane"></i>';
            }
        }

        async function deleteMessage(messageId, chatId) {
            try {
                // Delete from Telegram
                const response = await fetch(`https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/deleteMessage`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        chat_id: chatId,
                        message_id: messageId
                    })
                });
                
                const data = await response.json();
                
                if (data.ok) {
                    // Remove from local UI
                    const messageData = messageMap.get(messageId);
                    if (messageData) {
                        // Add delete animation
                        messageData.element.style.transform = 'scale(0.9)';
                        messageData.element.style.opacity = '0';
                        setTimeout(() => {
                            messageData.element.remove();
                        }, 200);
                        
                        messageMap.delete(messageId);
                    }
                } else {
                    throw new Error(data.description || "Failed to delete message");
                }
                
            } catch (error) {
                console.error("Error deleting message:", error);
                addSystemMessage("Failed to delete message: " + error.message);
            }
        }

        function resetInputState() {
            replyingToMessageId = null;
            editingMessageId = null;
            replyPreview.style.display = 'none';
            messageInput.style.height = 'auto';
        }

        function showContextMenu(e, messageId) {
            e.preventDefault();
            contextMenuMessageId = messageId;
            
            // Position the context menu
            contextMenu.style.display = 'flex';
            contextMenu.style.left = `${e.clientX}px`;
            contextMenu.style.top = `${e.clientY}px`;
            
            // Adjust if near window edges
            if (e.clientX + contextMenu.offsetWidth > window.innerWidth) {
                contextMenu.style.left = `${window.innerWidth - contextMenu.offsetWidth - 10}px`;
            }
            
            if (e.clientY + contextMenu.offsetHeight > window.innerHeight) {
                contextMenu.style.top = `${window.innerHeight - contextMenu.offsetHeight - 10}px`;
            }
        }

        function showMessageActions(messageElement, messageId) {
            // On mobile, show actions as a bottom sheet
            if (window.innerWidth <= 768) {
                // Create action sheet
                const actionSheet = document.createElement('div');
                actionSheet.style.position = 'fixed';
                actionSheet.style.bottom = '0';
                actionSheet.style.left = '0';
                actionSheet.style.right = '0';
                actionSheet.style.backgroundColor = 'var(--card)';
                actionSheet.style.borderRadius = 'var(--border-radius-md) var(--border-radius-md) 0 0';
                actionSheet.style.padding = 'var(--spacing-md)';
                actionSheet.style.boxShadow = 'var(--shadow-lg)';
                actionSheet.style.zIndex = '100';
                actionSheet.style.transform = 'translateY(100%)';
                actionSheet.style.transition = 'transform var(--transition)';
                
                // Get message data
                const messageData = messageMap.get(messageId);
                
                // Add buttons (only show edit for outgoing messages)
                actionSheet.innerHTML = `
                    <div style="display: flex; flex-direction: column; gap: var(--spacing-sm);">
                        <button class="action-btn reply-btn" style="justify-content: flex-start; padding: var(--spacing-sm); border-radius: var(--border-radius-sm);">
                            <i class="fas fa-reply" style="margin-right: var(--spacing-sm);"></i> Reply
                        </button>
                        ${messageData.isOutgoing ? `
                        <button class="action-btn edit-btn" style="justify-content: flex-start; padding: var(--spacing-sm); border-radius: var(--border-radius-sm);">
                            <i class="fas fa-edit" style="margin-right: var(--spacing-sm);"></i> Edit
                        </button>
                        ` : ''}
                        <button class="action-btn delete-btn" style="justify-content: flex-start; padding: var(--spacing-sm); border-radius: var(--border-radius-sm); color: var(--error);">
                            <i class="fas fa-trash-alt" style="margin-right: var(--spacing-sm);"></i> Delete
                        </button>
                        <button class="action-btn" style="justify-content: center; padding: var(--spacing-sm); margin-top: var(--spacing-md); border-radius: var(--border-radius-sm);" id="cancel-actions">
                            Cancel
                        </button>
                    </div>
                `;
                
                // Add to body
                document.body.appendChild(actionSheet);
                
                // Show with animation
                setTimeout(() => {
                    actionSheet.style.transform = 'translateY(0)';
                }, 10);
                
                // Add overlay
                const overlay = document.createElement('div');
                overlay.style.position = 'fixed';
                overlay.style.top = '0';
                overlay.style.left = '0';
                overlay.style.right = '0';
                overlay.style.bottom = '0';
                overlay.style.backgroundColor = 'rgba(0,0,0,0.5)';
                overlay.style.zIndex = '99';
                overlay.style.opacity = '0';
                overlay.style.transition = 'opacity var(--transition)';
                document.body.appendChild(overlay);
                
                // Show overlay
                setTimeout(() => {
                    overlay.style.opacity = '1';
                }, 10);
                
                // Handle cancel
                const cancelBtn = actionSheet.querySelector('#cancel-actions');
                const closeSheet = () => {
                    actionSheet.style.transform = 'translateY(100%)';
                    overlay.style.opacity = '0';
                    setTimeout(() => {
                        actionSheet.remove();
                        overlay.remove();
                    }, 300);
                };
                
                cancelBtn.addEventListener('click', closeSheet);
                overlay.addEventListener('click', closeSheet);
                
                // Handle actions
                const replyBtn = actionSheet.querySelector('.reply-btn');
                const editBtn = actionSheet.querySelector('.edit-btn');
                const deleteBtn = actionSheet.querySelector('.delete-btn');
                
                if (replyBtn) {
                    replyBtn.addEventListener('click', () => {
                        replyingToMessageId = messageId;
                        editingMessageId = null;
                        replyContent.textContent = messageData.element.querySelector('.message-content').textContent;
                        replyPreview.style.display = 'block';
                        messageInput.focus();
                        closeSheet();
                    });
                }
                
                if (editBtn) {
                    editBtn.addEventListener('click', () => {
                        editingMessageId = messageId;
                        replyingToMessageId = null;
                        messageInput.value = messageData.element.querySelector('.message-content').textContent;
                        messageInput.focus();
                        replyPreview.style.display = 'none';
                        closeSheet();
                    });
                }
                
                if (deleteBtn) {
                    deleteBtn.addEventListener('click', () => {
                        deleteMessage(messageId, messageData.chatId);
                        closeSheet();
                    });
                }
            }
        }

        function showTypingIndicator(sender) {
            const typingIndicator = document.createElement('div');
            typingIndicator.className = 'typing-indicator';
            typingIndicator.innerHTML = `
                <div class="typing-content">
                    <div class="typing-text">${sender} is typing</div>
                    <div class="typing-dots">
                        <div class="typing-dot"></div>
                        <div class="typing-dot"></div>
                        <div class="typing-dot"></div>
                    </div>
                </div>
            `;
            typingIndicator.id = 'typing-indicator';
            messagesContainer.appendChild(typingIndicator);
            scrollToBottom();
        }

        function hideTypingIndicator() {
            const typingIndicator = document.getElementById('typing-indicator');
            if (typingIndicator) {
                typingIndicator.remove();
            }
        }

        function updateTypingStatus(isTyping) {
            if (isTyping) {
                statusIndicator.className = 'status-indicator typing-status';
                statusText.textContent = 'typing...';
            } else {
                statusIndicator.className = 'status-indicator online';
                statusText.textContent = 'online';
            }
        }

        // UI Helper Functions
        function addSystemMessage(text) {
            addMessage("System", text, "system", null, new Date());
        }

        function addIncomingMessage(sender, text, messageId, date) {
            return addMessage(sender, text, "incoming", messageId, date);
        }

        function addOutgoingMessage(sender, text, messageId, date) {
            return addMessage(sender, text, "outgoing", messageId, date);
        }

        function addMessage(sender, text, type, messageId, date) {
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${type}`;
            
            if (messageId) {
                messageDiv.dataset.messageId = messageId;
            }
            
            const timeString = formatTime(date || new Date());
            
            messageDiv.innerHTML = `
                <div class="message-header">
                    <span class="sender-name">${sender}</span>
                    <span class="message-time">${timeString} ${type === 'outgoing' ? '<i class="fas fa-check"></i>' : ''}</span>
                </div>
                <div class="message-content">
                    ${escapeHtml(text)}
                </div>
                <div class="message-actions">
                    <button class="action-btn reply-btn" title="Reply">
                        <i class="fas fa-reply"></i>
                    </button>
                    ${type === 'outgoing' ? `
                    <button class="action-btn edit-btn" title="Edit">
                        <i class="fas fa-edit"></i>
                    </button>
                    ` : ''}
                    <button class="action-btn delete-btn" title="Delete">
                        <i class="fas fa-trash-alt"></i>
                    </button>
                </div>
            `;
            
            // Add action handlers
            const replyBtn = messageDiv.querySelector('.reply-btn');
            const editBtn = messageDiv.querySelector('.edit-btn');
            const deleteBtn = messageDiv.querySelector('.delete-btn');
            
            replyBtn?.addEventListener('click', (e) => {
                e.stopPropagation();
                replyingToMessageId = messageId;
                editingMessageId = null;
                replyContent.textContent = text;
                replyPreview.style.display = 'block';
                messageInput.focus();
            });
            
            editBtn?.addEventListener('click', (e) => {
                e.stopPropagation();
                editingMessageId = messageId;
                replyingToMessageId = null;
                messageInput.value = text;
                messageInput.focus();
                replyPreview.style.display = 'none';
            });
            
            deleteBtn?.addEventListener('click', (e) => {
                e.stopPropagation();
                if (messageId) {
                    const messageData = messageMap.get(messageId);
                    if (messageData) {
                        deleteMessage(messageId, messageData.chatId);
                    }
                } else {
                    messageDiv.remove();
                }
            });
            
            messagesContainer.appendChild(messageDiv);
            scrollToBottom();
            
            return messageDiv;
        }

        function formatTime(date) {
            return date.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', hour12: true });
        }

        function escapeHtml(text) {
            const div = document.createElement('div');
            div.textContent = text;
            return div.innerHTML;
        }

        function updateConnectionStatus(connected, text) {
            isConnected = connected;
            connectionStatus.className = `status-indicator ${connected ? 'online' : 'offline'}`;
            connectionText.textContent = text;
            
            // Update chat header status
            statusIndicator.className = `status-indicator ${connected ? 'online' : 'offline'}`;
            statusText.textContent = connected ? "online" : "offline";
        }

        function updateLastUpdated() {
            const now = new Date();
            lastUpdated.textContent = "Last updated: " + now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', hour12: true });
        }

        function scrollToBottom() {
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }

        // Event Listeners
        sendBtn.addEventListener('click', async () => {
            const message = messageInput.value.trim();
            if (message) {
                await sendMessageToTelegram(message);
            }
        });

        messageInput.addEventListener('keydown', async (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                const message = messageInput.value.trim();
                if (message) {
                    await sendMessageToTelegram(message);
                }
            }
        });

        // Initialize with a clean state
        setTimeout(() => {
            // Clear any initial system messages
            while (messagesContainer.firstChild) {
                messagesContainer.removeChild(messagesContainer.firstChild);
            }
        }, 100);
    </script>
</body>
</html>
