# MIND-VERSE-SIMULATOR-GAME
KOTLIN
import kotlin.random.Random
import kotlin.system.exitProcess

// ---------------- ENUMS ----------------
enum class Emotion {
    HAPPY, SAD, ANGRY, CONFUSED, MOTIVATED, EMPTY
}

enum class Action {
    STUDY, SLEEP, THINK, WALK, HELP_OTHERS, DO_NOTHING
}

enum class Event {
    SUCCESS, FAILURE, LOSS, DISCOVERY, REALIZATION, SILENCE
}

// ---------------- DATA CLASSES ----------------
data class Mind(
    var energy: Int = 50,
    var clarity: Int = 50,
    var emotion: Emotion = Emotion.CONFUSED,
    var karma: Int = 0
)

data class LifeClock(
    var day: Int = 1,
    var hour: Int = 6
)

// ---------------- CORE SIMULATOR ----------------
class MindVerse {
    private val mind = Mind()
    private val clock = LifeClock()
    private var alive = true

    fun start() {
        printIntro()

        while (alive) {
            showStatus()
            val action = chooseAction()
            processAction(action)
            triggerRandomEvent()
            updateClock()
            checkEnd()
        }
    }

    private fun printIntro() {
        println("====================================")
        println("        MINDVERSE SIMULATOR")
        println("====================================")
        println("You are not controlling a body.")
        println("You are controlling a MIND.")
        println("Every choice shapes your inner world.\n")
    }

    private fun showStatus() {
        println("\n------ DAY ${clock.day} | HOUR ${clock.hour}:00 ------")
        println("Energy  : ${mind.energy}")
        println("Clarity : ${mind.clarity}")
        println("Emotion : ${mind.emotion}")
        println("Karma   : ${mind.karma}")
    }

    private fun chooseAction(): Action {
        println("\nChoose an action:")
        Action.values().forEachIndexed { index, action ->
            println("${index + 1}. $action")
        }

        print("Your choice: ")
        val input = readLine()?.toIntOrNull()

        return if (input != null && input in 1..Action.values().size) {
            Action.values()[input - 1]
        } else {
            println("Indecision consumes energy...")
            Action.DO_NOTHING
        }
    }

    private fun processAction(action: Action) {
        when (action) {
            Action.STUDY -> {
                mind.energy -= 10
                mind.clarity += 15
                mind.emotion = Emotion.MOTIVATED
                mind.karma += 1
                println("You studied deeply. Knowledge expanded.")
            }

            Action.SLEEP -> {
                mind.energy += 20
                mind.clarity -= 5
                mind.emotion = Emotion.EMPTY
                println("You slept. Dreams whispered truths.")
            }

            Action.THINK -> {
                mind.energy -= 5
                mind.clarity += 10
                mind.emotion = Emotion.CONFUSED
                println("You questioned reality itself.")
            }

            Action.WALK -> {
                mind.energy += 5
                mind.clarity += 5
                mind.emotion = Emotion.HAPPY
                println("You walked silently. Nature listened.")
            }

            Action.HELP_OTHERS -> {
                mind.energy -= 5
                mind.karma += 10
                mind.emotion = Emotion.HAPPY
                println("You helped someone without being seen.")
            }

            Action.DO_NOTHING -> {
                mind.energy -= 2
                mind.clarity -= 2
                mind.emotion = Emotion.EMPTY
                println("Nothing happened. Time still passed.")
            }
        }

        normalizeStats()
    }

    private fun triggerRandomEvent() {
        val chance = Random.nextInt(100)

        if (chance < 30) {
            val event = Event.values().random()
            when (event) {
                Event.SUCCESS -> {
                    mind.clarity += 10
                    mind.emotion = Emotion.HAPPY
                    println("EVENT: Unexpected success boosted your confidence.")
                }

                Event.FAILURE -> {
                    mind.energy -= 10
                    mind.emotion = Emotion.SAD
                    println("EVENT: Failure taught a painful lesson.")
                }

                Event.LOSS -> {
                    mind.clarity -= 5
                    mind.emotion = Emotion.ANGRY
                    println("EVENT: You lost something important.")
                }

                Event.DISCOVERY -> {
                    mind.clarity += 20
                    println("EVENT: A deep realization struck you.")
                }

                Event.REALIZATION -> {
                    mind.karma += 5
                    println("EVENT: You understood yourself better.")
                }

                Event.SILENCE -> {
                    println("EVENT: Nothing happened. Silence spoke volumes.")
                }
            }
        }
    }

    private fun updateClock() {
        clock.hour += 3
        if (clock.hour >= 24) {
            clock.hour = 0
            clock.day++
        }
    }

    private fun normalizeStats() {
        mind.energy = mind.energy.coerceIn(0, 100)
        mind.clarity = mind.clarity.coerceIn(0, 100)
        mind.karma = mind.karma.coerceAtLeast(0)
    }

    private fun checkEnd() {
        if (mind.energy <= 0) {
            println("\nYour mind is exhausted.")
            println("You fade into deep sleep...")
            alive = false
        }

        if (mind.clarity >= 100) {
            println("\nYour mind reached absolute clarity.")
            println("You achieved INNER AWAKENING.")
            alive = false
        }

        if (clock.day > 30) {
            println("\n30 days passed.")
            println("Life moves on. Reflection remains.")
            alive = false
        }
    }
}

// ---------------- MAIN ----------------
fun main() {
    val simulator = MindVerse()
    simulator.start()

    println("\nThank you for exploring MindVerse.")
    exitProcess(0)
}
