package com.example.halebeeduchatbot

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                Surface(modifier = Modifier.fillMaxSize(), color = MaterialTheme.colorScheme.background) {
                    ChatScreen()
                }
            }
        }
    }
}

@Composable
fun ChatScreen() {
    var userInput by remember { mutableStateOf("") }
    val messages = remember { mutableStateListOf("🤖 Welcome! Ask me about Halebeedu Temple.") }
    
    Column(modifier = Modifier.fillMaxSize().padding(16.dp)) {
        LazyColumn(modifier = Modifier.weight(1f)) {
            items(messages) { message ->
                Text(text = message, fontSize = 18.sp, modifier = Modifier.padding(8.dp))
            }
        }
        
        Row(verticalAlignment = Alignment.CenterVertically) {
            TextField(
                value = userInput,
                onValueChange = { userInput = it },
                label = { Text("Type your question") },
                modifier = Modifier.weight(1f)
            )
            Spacer(modifier = Modifier.width(8.dp))
            Button(onClick = {
                if (userInput.isNotBlank()) {
                    messages.add(" You: $userInput")
                    val response = getBotResponse(userInput)
                    messages.add(" Bot: $response")
                    userInput = ""
                }
            }) {
                Text("Send")
            }
        }
    }
}

fun getBotResponse(input: String): String {
    val lower = input.lowercase()
    
    return when {
        // English responses
        "history" in lower -> "Halebeedu Temple was built in the 12th century by the Hoysala dynasty."
        "location" in lower -> "It is located in Hassan district, Karnataka."
        "deity" in lower || "god" in lower -> "The temple is dedicated to Lord Hoysaleswara and Shantaleswara."
        "architecture" in lower -> "It features intricate soapstone carvings and twin shrines."
        "timing" in lower || "open" in lower -> "The temple is open from 9 AM to 6 PM every day."
        
        
        else -> "Sorry, I don't have information about that yet. Try asking about history, location, or architecture."
    }
}
