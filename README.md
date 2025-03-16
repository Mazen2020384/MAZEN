import pygame
import random

# Initialize pygame
pygame.init()

# Game settings
WIDTH, HEIGHT = 800, 600
FPS = 30
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Echoes of Eternity")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)

# Fonts
font = pygame.font.SysFont("Arial", 24)

# Player Class
class Character:
    def __init__(self, name, hp, attack, defense, elemental_power):
        self.name = name
        self.hp = hp
        self.max_hp = hp
        self.attack = attack
        self.defense = defense
        self.elemental_power = elemental_power

    def take_damage(self, damage):
        self.hp -= damage
        if self.hp < 0:
            self.hp = 0

    def heal(self, amount):
        self.hp += amount
        if self.hp > self.max_hp:
            self.hp = self.max_hp

    def attack_enemy(self, enemy):
        damage = max(self.attack - enemy.defense, 1)  # Basic damage formula
        enemy.take_damage(damage)
        return damage

    def use_elemental_power(self, enemy):
        damage = random.randint(15, 30)  # Elemental attack range
        enemy.take_damage(damage)
        return damage


# Dialogue System
def show_dialogue(text):
    dialogue_surface = pygame.Surface((WIDTH - 40, 100))
    dialogue_surface.fill(BLACK)
    screen.blit(dialogue_surface, (20, HEIGHT - 120))
    
    text_surface = font.render(text, True, WHITE)
    screen.blit(text_surface, (30, HEIGHT - 100))


# Combat System
def combat(player, enemy):
    in_combat = True
    while in_combat:
        screen.fill(WHITE)
        
        # Display player and enemy stats
        player_health_text = font.render(f"{player.name} HP: {player.hp}/{player.max_hp}", True, GREEN)
        enemy_health_text = font.render(f"{enemy.name} HP: {enemy.hp}/{enemy.max_hp}", True, RED)
        
        screen.blit(player_health_text, (20, 20))
        screen.blit(enemy_health_text, (WIDTH - enemy_health_text.get_width(
