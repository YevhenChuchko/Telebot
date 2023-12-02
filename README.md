# Telebot
// main.go
package main

import (
	"fmt"
	"github.com/spf13/cobra"
	"gopkg.in/telebot.v3"
)

var token string

var rootCmd = &cobra.Command{
	Use:   "my_telegram_bot",
	Short: "My Telegram Bot",
	Run: func(cmd *cobra.Command, args []string) {
		bot, err := telebot.NewBot(telebot.Settings{
			Token: token,
		})
		if err != nil {
			panic(err)
		}

		bot.Handle(telebot.OnText, func(m *telebot.Message) {
			bot.Send(m.Chat, fmt.Sprintf("You said: %s", m.Text))
		})

		bot.Start()
	},
}

func init() {
	rootCmd.Flags().StringVarP(&token, "token", "t", "", "Telegram bot token")
	rootCmd.MarkFlagRequired("token")
}

func main() {
	if err := rootCmd.Execute(); err != nil {
		panic(err)
	}
}

