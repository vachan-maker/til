> [!WARNING]
> This content is AI-generated. Verify before relying on it.

# Task Scheduling

Laravel's task scheduler eliminates the need to configure multiple cron entries on your production server. Instead, a single cron entry calls Artisan every minute, and all schedules are managed cleanly in application code.

## Server Crontab Setup

Add this single cron job to your production server via `crontab -e`:

```bash
* * * * * cd /path-to-your-project && php artisan schedule:run >> /dev/null 2>&1
```

---

## Defining Schedules

### Laravel 11+ (`routes/console.php`)

```php
use App\Jobs\GenerateWeeklyReport;
use Illuminate\Support\Facades\Schedule;

// Schedule an Artisan command
Schedule::command('backup:clean')->dailyAt('01:00');
Schedule::command('backup:run')->dailyAt('02:00');

// Schedule a queued job
Schedule::job(new GenerateWeeklyReport)->mondays()->at('06:00');

// Schedule a closure / PHP callback
Schedule::call(function () {
    \DB::table('temp_uploads')->where('created_at', '<', now()->subDay())->delete();
})->hourly();

// Schedule an OS shell command
Schedule::exec('node /scripts/metrics.js')->everyTenMinutes();
```

### Laravel 10 and Earlier (`app/Console/Kernel.php`)

```php
namespace App\Console;

use Illuminate\Console\Scheduling\Schedule;
use Illuminate\Foundation\Console\Kernel as ConsoleKernel;

class Kernel extends ConsoleKernel
{
    protected function schedule(Schedule $schedule): void
    {
        $schedule->command('inspire')->hourly();
    }
}
```

---

## Frequency Schedule Methods

| Schedule Method | Execution Timing |
| --- | --- |
| `->everyMinute();` | Runs once every minute |
| `->everyFiveMinutes();` | Runs every 5 minutes (`0, 5, 10, ...`) |
| `->everyTenMinutes();` | Runs every 10 minutes |
| `->everyFifteenMinutes();` | Runs every 15 minutes |
| `->everyThirtyMinutes();` | Runs every 30 minutes |
| `->hourly();` | Runs at the beginning of every hour |
| `->hourlyAt(15);` | Runs at 15 minutes past the hour |
| `->daily();` | Runs daily at midnight (`00:00`) |
| `->dailyAt('14:30');` | Runs daily at specified 24h time (`14:30`) |
| `->twiceDaily(1, 13);` | Runs twice daily at specified hours (`01:00` and `13:00`) |
| `->weekly();` | Runs weekly on Sunday at midnight |
| `->weeklyOn(1, '08:00');` | Runs weekly on Monday (`1`) at `08:00` |
| `->monthly();` | Runs monthly on the 1st at midnight |
| `->monthlyOn(15, '10:00');` | Runs on the 15th of every month at `10:00` |
| `->weekdays();` | Restricts execution to Monday through Friday |
| `->weekends();` | Restricts execution to Saturday and Sunday |
| `->timezone('UTC');` | Overrides the default timezone for the task |
| `->cron('0 */4 * * *');` | Custom standard cron expression |

---

## Execution Modifiers & Constraints

```php
// Prevent task overlap if the previous execution is still running
Schedule::command('emails:send')
    ->everyMinute()
    ->withoutOverlapping(); // Optional: ->withoutOverlapping(10) sets a 10 min lock expiry

// Run task in the background so it does not block subsequent tasks
Schedule::command('reports:compile')
    ->daily()
    ->runInBackground();

// Multi-server clustering: Ensure the task runs on only one server instance
Schedule::command('queue:prune-batches')
    ->daily()
    ->onOneServer();

// Run even when the app is in 'php artisan down' maintenance mode
Schedule::command('health:check')
    ->everyFiveMinutes()
    ->evenInMaintenanceMode();

// Conditional execution
Schedule::command('reminders:send')
    ->daily()
    ->when(fn () => config('features.reminders_enabled'));
```

---

## Testing & Inspecting Schedules

| Command | Description |
| --- | --- |
| `php artisan schedule:list` | Displays a table of all scheduled tasks, intervals, and next run time |
| `php artisan schedule:run` | Evaluates and triggers any tasks due at the current minute |
| `php artisan schedule:work` | Runs a foreground daemon locally that fires `schedule:run` every minute |

> [!TIP]
> Use `php artisan schedule:work` during local development to test scheduled jobs without touching your local OS crontab.
