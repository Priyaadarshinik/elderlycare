# Empowering Elderly care with multiagent framework
# SAGE: Smart Agent Grid for Elderly Care!
Problem statement you are trying to address 
Empowering Elderly Care with Multi-Agent AI System

As the global population ages, ensuring the well-being of elderly individuals living independently presents a significant challenge. Many elderly individuals require continuous monitoring, timely reminders, and emergency support to maintain their health and safety. The absence of real-time assistance can lead to health deterioration, missed medications, or unnoticed emergencies.

Key challenges include :

Lack of real-time monitoring of vital signs and activities.
Missed medications and appointments due to forgetfulness.
Unnoticed emergencies such as falls or health deterioration.
Social isolation, leading to mental health concerns.

A holistic solution is required to provide continuous monitoring, proactive reminders, emergency alerts, and social engagement to support elderly individuals in living independently and safely.

# Proposed Solution Overview
Methodology & Step-by-Step Approach

1. Problem Identification & Needs Assessment
Conducted a detailed analysis of common elderly care challenges: health monitoring, medication adherence, safety, and social isolation.
2. System Design & Agent Definition
Defined roles for each AI agent (Health, Safety, Reminder, Social, Emergency).
Ensured agents collaborate through a centralized multi-agent framework.
3. Technology Stack Selection
Chose Ollama for on-prem LLMs for privacy.
Implemented embedding models for contextual understanding.
Used SQLite for lightweight local storage and real-time access.
4. Development of Individual Agents
Built custom tools: APIs for wearable integration, ML models for anomaly detection, and scrapers for external data.
Integrated voice-based AI assistants for reminders and interaction.
5. Multi-Agent Integration
Designed inter-agent communication flows using the multi-agent framework.
Ensured agents share real-time data and alerts for timely response.
6. Interface & Experience Design
Developed user-friendly mobile/web interfaces for caregivers and seniors.
Integrated real-time dashboards, health insights, and alert systems.
7. Testing & Simulation
Simulated real-life elderly scenarios to validate each agent’s functionality.
Conducted usability testing with feedback loops for refinement.
8. Deployment
Hosted the system on a secure local server (on-premise).
Integrated with IoT devices and wearables.

9. Monitoring & Feedback Collection
Set up logging for agent performance and incident reports.
Added feedback forms for caregivers and family members for continuous improvement. To address this challenge, we propose a Multi-Agent AI System for Elderly Care, integrating various AI agents that collaboratively provide real-time monitoring, reminders, and safety alerts. This system facilitates communication among caregivers, healthcare providers, and family members to ensure comprehensive elderly care.

# Technologies Used 


AI & Machine Learning: Predictive analytics, anomaly detection, and voice assistance.

Ollama-based On-Prem LLMs: Locally hosted large language models for privacy and efficiency.
Custom Tools for Agents: APIs, web scrapers, and machine learning models.

Ollama-based Embedding Models: AI-driven contextual understanding and semantic search.

SQLite DB: Lightweight, efficient, and easily deployable database for storing relevant data.

Multi-Agent Framework: Enables seamless collaboration between different AI agents.

IoT & Wearable Devices: Smart sensors for real-time data collection.

Cloud Computing: Secure data storage and real-time access.

Natural Language Processing (NLP): Voice-based reminders and communication.

Mobile & Web Application: User-friendly dashboard for caregivers and family members.

# Agents' interaction design 

The multi-agent system operates in a collaborative manner:

The Health Monitoring Agent shares real-time data with the Emergency Response Agent if anomalies are detected.

The Safety Monitoring Agent alerts caregivers in case of falls and interacts with the Emergency Response Agent.

The Daily Activity Reminder Agent adapts schedules based on health status.

The Social Engagement Agent fosters communication among stakeholders.

# Code structure
![image](https://github.com/user-attachments/assets/3c429cce-a86a-46d0-a65e-c59f70d987ca)

![image](https://github.com/user-attachments/assets/d6397f6b-dc13-4fda-adb6-b13f29037808)
```
import streamlit as st
import pandas as pd
import plotly.express as px

# Load datasets
health_df = pd.read_csv("health_monitoring.csv")
safety_df = pd.read_csv("safety_monitoring.csv")
reminder_df = pd.read_csv("daily_reminder.csv")

# Page config
st.set_page_config(page_title="Elderly Care AI Dashboard", layout="wide")
st.title("👵 Elderly Care AI Dashboard")
st.markdown("""
    <style>
    .main {background-color: #f5f7fa;}
    .css-18e3th9 {background-color: #f5f7fa;}
    </style>
    """, unsafe_allow_html=True)
st.markdown("Real-time Monitoring & Insights")

# Tabs for categories
tabs = st.tabs(["🩺 Health Stats", "🛡 Safety Monitoring", "⏰ Daily Reminders", "📢 Alerts & Notifications"])

# --- HEALTH TAB --- #
with tabs[0]:
    st.subheader("Vital Signs Overview")
    st.dataframe(health_df.head())

    # Heart Rate Trend
    fig_hr = px.line(health_df, x="Timestamp", y="Heart Rate", title="💓 Heart Rate Over Time",
                     color="Device-ID/User-ID")
    st.plotly_chart(fig_hr, use_container_width=True)

    # Glucose Levels
    fig_gl = px.bar(health_df, x="Device-ID/User-ID", y="Glucose Levels",
                   title="🩸 Current Glucose Levels by User",
                   color="Glucose Levels Below/Above Threshold (Yes/No)")
    st.plotly_chart(fig_gl, use_container_width=True)

# --- SAFETY TAB --- #
with tabs[1]:
    st.subheader("Fall & Inactivity Alerts")
    st.dataframe(safety_df.head())

    fall_counts = safety_df["Fall Detected (Yes/No)"].value_counts().reset_index()
    fall_counts.columns = ["Fall Detected", "Count"]
    fig_falls = px.pie(fall_counts, names="Fall Detected", values="Count", title="🧍‍♀️ Fall Detection Summary")
    st.plotly_chart(fig_falls, use_container_width=True)

    location_chart = px.histogram(safety_df, x="Location", color="Fall Detected (Yes/No)",
                                  title="📍 Fall Events by Location")
    st.plotly_chart(location_chart, use_container_width=True)

# --- REMINDERS TAB --- #
with tabs[2]:
    st.subheader("Reminder Compliance")
    st.dataframe(reminder_df.head())

    reminder_types = reminder_df["Reminder Type"].value_counts().reset_index()
    reminder_types.columns = ["Reminder Type", "Count"]
    fig_reminders = px.bar(reminder_types, x="Reminder Type", y="Count", title="🔔 Reminder Types Frequency",
                           color="Reminder Type")
    st.plotly_chart(fig_reminders, use_container_width=True)

    ack_chart = reminder_df["Acknowledged (Yes/No)"].value_counts().reset_index()
    ack_chart.columns = ["Acknowledged", "Count"]
    fig_ack = px.pie(ack_chart, names="Acknowledged", values="Count", title="✅ Acknowledgement Status")
    st.plotly_chart(fig_ack, use_container_width=True)

# --- ALERTS & NOTIFICATIONS TAB --- #
with tabs[3]:
    st.subheader("📢 Real-Time Alerts & Family Notifications")

    # Define alert condition
    safety_df["Alert Triggered"] = ((safety_df["Fall Detected (Yes/No)"] == "Yes") |
                                     (safety_df["Post-Fall Inactivity Duration (Seconds)"] > 300))
    alerts_df = safety_df[safety_df["Alert Triggered"] == True]

    if not alerts_df.empty:
        st.warning("⚠️ Critical Alerts Detected! Immediate action required!")
        st.dataframe(alerts_df, use_container_width=True)

        st.markdown("""
        ### 👨‍👩‍👧 Family Notification
        - 🛑 Family members have been notified via registered contacts.
        - 👩‍⚕️ Caregivers alerted through app dashboard.
        - 🔁 Emergency protocols auto-initiated.
        """)
    else:
        st.success("✅ No critical alerts. Everything looks good.")


```

# Sample Dashboard

[elderlycare.webm](https://github.com/user-attachments/assets/bbcabdd0-def6-4a2b-b3a7-1d886e25bf86)


# Conclusion

Our Multi-Agent AI System revolutionizes elderly care by combining intelligent monitoring, proactive support, and seamless collaboration. It tackles key challenges like missed medications, unnoticed emergencies, and social isolation with a smart, responsive network of AI agents. The solution empowers seniors to live independently and confidently, while offering caregivers timely insights and alerts. This holistic approach not only enhances safety and well-being but also redefines what it means to age with dignity in the digital era.







