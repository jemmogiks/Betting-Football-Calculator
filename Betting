# Football Betting Calculator (Web-based Version)
# This script creates an interactive web app for football betting predictions.

import streamlit as st
import pandas as pd

def calculate_probabilities(team1, team2, weight_factor=1.0):
    """Calculates probabilities based on weighted team stats."""
    try:
        team1_win_prob = (team1['Matches Won'] / team1['Matches Played']) * weight_factor
        team2_win_prob = (team2['Matches Won'] / team2['Matches Played']) * weight_factor
        draw_prob = max(0, 1 - (team1_win_prob + team2_win_prob))
        return team1_win_prob, draw_prob, team2_win_prob
    except ZeroDivisionError:
        return 0, 1, 0  # Default to a draw if division by zero occurs

def expected_goals(team):
    """Calculates expected goals per match."""
    try:
        return team['Goals Scored'] / team['Matches Played']
    except ZeroDivisionError:
        return 0

def simulate_profit(odds, bet_amount, outcome):
    """Simulates profit based on betting odds and outcome."""
    if outcome == 'win':
        return bet_amount * odds - bet_amount
    return -bet_amount

def bet_recommendation(team1_prob, draw_prob, team2_prob, threshold=0.4):
    """Recommends the best bet based on probability threshold."""
    if team1_prob > threshold:
        return "Bet on Team 1"
    elif team2_prob > threshold:
        return "Bet on Team 2"
    elif draw_prob > threshold:
        return "Bet on Draw"
    return "No Clear Bet"

# Streamlit UI
st.title("Football Betting Calculator")

# User inputs
team1_matches = st.number_input("Team 1 Matches Played", min_value=1, value=30)
team1_wins = st.number_input("Team 1 Matches Won", min_value=0, value=12)
team1_goals = st.number_input("Team 1 Goals Scored", min_value=0, value=22)

team2_matches = st.number_input("Team 2 Matches Played", min_value=1, value=30)
team2_wins = st.number_input("Team 2 Matches Won", min_value=0, value=22)
team2_goals = st.number_input("Team 2 Goals Scored", min_value=0, value=54)

weight_factor = st.slider("Weight Adjustment Factor", 0.5, 2.0, 1.2)

# Calculate probabilities
team1 = {'Matches Played': team1_matches, 'Matches Won': team1_wins, 'Goals Scored': team1_goals}
team2 = {'Matches Played': team2_matches, 'Matches Won': team2_wins, 'Goals Scored': team2_goals}

team1_prob, draw_prob, team2_prob = calculate_probabilities(team1, team2, weight_factor)
team1_exp_goals = expected_goals(team1)
team2_exp_goals = expected_goals(team2)

bet_suggestion = bet_recommendation(team1_prob, draw_prob, team2_prob)

# Betting Simulation
bet_amount = st.number_input("Bet Amount", min_value=1, value=100)
odds = st.number_input("Odds for Selected Team", min_value=1.1, value=2.5)
outcome = st.selectbox("Bet Outcome", ["win", "lose"])
profit = simulate_profit(odds, bet_amount, outcome)

# Display results
st.subheader("Calculated Probabilities")
st.write(f"Team 1 Win Probability: {team1_prob:.2f}")
st.write(f"Draw Probability: {draw_prob:.2f}")
st.write(f"Team 2 Win Probability: {team2_prob:.2f}")

st.subheader("Expected Goals")
st.write(f"Team 1 Expected Goals: {team1_exp_goals:.2f}")
st.write(f"Team 2 Expected Goals: {team2_exp_goals:.2f}")

st.subheader("Bet Recommendation")
st.write(bet_suggestion)

st.subheader("Profit Simulation")
st.write(f"Profit from Bet: {profit:.2f}")
