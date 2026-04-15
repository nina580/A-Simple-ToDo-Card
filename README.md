import React, { useEffect, useState } from "react";

function getTimeRemaining(dueDate) {
  const now = new Date();
  const diff = dueDate - now;

  const minutes = Math.floor(diff / (1000 * 60));
  const hours = Math.floor(diff / (1000 * 60 * 60));
  const days = Math.floor(diff / (1000 * 60 * 60 * 24));

  if (diff <= 0 && Math.abs(minutes) < 1) return "Due now!";
  if (diff < 0) {
    if (Math.abs(hours) < 1) return `Overdue by ${Math.abs(minutes)} minutes`;
    if (Math.abs(days) < 1) return `Overdue by ${Math.abs(hours)} hours`;
    return `Overdue by ${Math.abs(days)} days`;
  }

  if (days >= 1) return `Due in ${days} day${days > 1 ? "s" : ""}`;
  if (hours >= 1) return hours === 24 ? "Due tomorrow" : `Due in ${hours} hours`;
  return `Due in ${minutes} minutes`;
}

export default function TodoCard() {
  const [completed, setCompleted] = useState(false);
  const [timeRemaining, setTimeRemaining] = useState("");

  const dueDate = new Date(Date.now() + 1000 * 60 * 60 * 26); // ~26 hours from now

  useEffect(() => {
    const update = () => {
      setTimeRemaining(getTimeRemaining(dueDate));
    };

    update();
    const interval = setInterval(update, 60000);
    return () => clearInterval(interval);
  }, []);

  const status = completed ? "Done" : "Pending";
  const priority = "High";

  return (
    <div className="min-h-screen flex items-center justify-center p-4 bg-gray-100">
      <article
        data-testid="test-todo-card"
        className="w-full max-w-md bg-white rounded-2xl shadow-md p-5 flex flex-col gap-4"
      >
        {Header}
        <div className="flex items-start justify-between gap-3">
          <div className="flex items-start gap-3">
            <input
              id="todo-complete"
              type="checkbox"
              data-testid="test-todo-complete-toggle"
              checked={completed}
              onChange={() => setCompleted(!completed)}
              className="mt-1 w-5 h-5"
            />

            <div>
              <h2
                data-testid="test-todo-title"
                className={`text-lg font-semibold ${
                  completed ? "line-through text-gray-400" : ""
                }`}
              >
                Do Laundry
              </h2>

              <p
                data-testid="test-todo-description"
                className="text-sm text-gray-600"
              >
                Remember to carry fabric softener.
              </p>
            </div>
          </div>

          <span
            data-testid="test-todo-priority"
            className="text-xs font-medium px-2 py-1 rounded-full bg-red-100 text-red-600"
            aria-label="High priority"
          >
            High
          </span>
        </div>

        {Tags}
        <ul
          data-testid="test-todo-tags"
          className="flex flex-wrap gap-2"
          role="list"
        >
          <li
            data-testid="test-todo-tag-work"
            className="text-xs bg-gray-200 px-2 py-1 rounded-full"
          >
            Laundry
          </li>
          <li
            data-testid="test-todo-tag-urgent"
            className="text-xs bg-yellow-200 px-2 py-1 rounded-full"
          >
            urgent
          </li>
          <li className="text-xs bg-blue-200 px-2 py-1 rounded-full">
            cleaning
          </li>
        </ul>

        {Dates}
        <div className="text-sm text-gray-700 flex flex-col gap-1">
          <time
            data-testid="test-todo-due-date"
            dateTime={dueDate.toISOString()}
          >
            Due {dueDate.toLocaleDateString(undefined, {
              month: "short",
              day: "numeric",
              year: "numeric",
            })}
          </time>

          <time
            data-testid="test-todo-time-remaining"
            aria-live="polite"
          >
            {timeRemaining}
          </time>
        </div>

        {Status}
        <span
          data-testid="test-todo-status"
          className="text-sm font-medium"
          aria-label={`Status: ${status}`}
        >
          {status}
        </span>

        {Actions}
        <div className="flex gap-3">
          <button
            data-testid="test-todo-edit-button"
            onClick={() => console.log("edit clicked")}
            className="flex-1 px-3 py-2 rounded-lg bg-gray-200 hover:bg-gray-300 focus:outline-none focus:ring-2 focus:ring-black"
          >
            Edit
          </button>

          <button
            data-testid="test-todo-delete-button"
            onClick={() => alert("Delete clicked")}
            className="flex-1 px-3 py-2 rounded-lg bg-red-500 text-white hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-black"
          >
            Delete
          </button>
        </div>
      </article>
    </div>
  );
}
