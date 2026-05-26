# Task: Serve an ML Model with FastAPI

The xFusionCorp Industries ML platform team exposes the `fraud-detection` model through a FastAPI service and uses the auto-generated Swagger UI at `/docs` as the primary interactive surface—engineers submit predictions directly from the browser rather than crafting curl invocations. The FastAPI server is already running on port `8085` against the pre-trained model, and every prediction it serves is recorded in an in-memory audit log. Your task is to submit three distinct fraud-detection predictions through the Swagger UI's `Try it out` panel and confirm the server recorded every one.


1. The FastAPI server is already running on port `8085`. The FastAPI Swagger UI button at the top of the lab opens `/docs` — the interactive Swagger surface.

2. The project layout under `/root/code/serving/`:

  - `model.pkl` – Deterministic RandomForest trained at startup on the shared `amount / hour / num_tx_past_day → is_fraud` synthetic dataset.
  - `train.csv` – The 10-row source used to fit the model.
  - `app.py` – FastAPI application wired with `/health`, `POST /predict`, and `GET /last-predictions`. Correct and must remain intact — this lab does not edit the code.

3. Open the FastAPI Swagger UI button, expand the `POST /predict` section, click `Try it out`, enter a payload in the editable JSON panel, click `Execute`, and observe the response. Repeat three times with three distinct (`amount`, `hour`, `num_tx_past_day`) combinations.

4. The end state must include:

  - The Swagger UI at `/docs` is reachable (the FastAPI Swagger UI button loads it).
  - `GET http://localhost:8085/last-predictions` returns a JSON object whose count is at least 3 and whose predictions array carries at least three distinct (`amount`, `hour`, `num_tx_past_day`) tuples.
  - Every recorded prediction carries an `is_fraud` flag of `0` or `1`.

> Suggested payloads to exercise both classes: `{"amount": 3200, "hour": 23, "num_tx_past_day": 5}` (high-value, late-night—expected to flag fraud); `{"amount": 25.5, "hour": 10, "num_tx_past_day": 1}` (low-value, daytime—expected to pass); `{"amount": 890, "hour": 2, "num_tx_past_day": 3}` (mid-range borderline).

---

# Solution:

- This task involves invoking the `/predict` endpoint from the Swagger UI with three distinct fraud-detection prediction payloads.

- Change into the `serving` directory and inspect `./app.py`. The server should be functional. Start it with `python3 ./app.py &`:

![run-app.py](./assets/mlops-day58.png)

- Open the Swagger UI by clicking the button on top-right of the lab terminal.

![open-swagger-ui](./assets/mlops-day58a.png)

- Check the `/health` endpoint to confirm the server is running:

![check-health](./assets/mlops-day58b.png)

- Now invoke the `/predict` endpoint with each of the following payloads:
  - `{"amount": 3200, "hour": 23, "num_tx_past_day": 5}`
  - `{"amount": 25.5, "hour": 10, "num_tx_past_day": 1}`
  - `{"amount": 890, "hour": 2, "num_tx_past_day": 3}`

Click **Try it out**, update the values accordingly, and click **Execute** for all three predictions. Each should return a different `is_fraud` value.

![check-predcit](./assets/mlops-day58c.png)

- Now in the lab terminal, query the `/last-predictions` endpoint. It should return the same three predictions in an array with the values provided via the Swagger UI:

```
curl http://localhost:8085/last-predictions
```

![last-rediction](./assets/mlops-day58d.png)

Hit **Check**


