# Elastic Search routines
<code>
  GET transactions/_search

POST _reindex
{
  "source": {
    "index": "transactions"
  },
  "dest": {
    "index": "transaction_part1"
  },
  "script": {
    "source": """
      ctx._source.transactionDate = new Date(ctx._source.transactionDate).toString();
    """
  }
}

GET transaction_part1/_search

POST _reindex
{
  "source": {
    "index": "transactions"
  },
  "dest": {
    "index": "transaction_new"
  },
  "script": {
    "source": """
      SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss");
      formatter.setTimeZone(TimeZone.getTimeZone('UTC'));
      ctx._source.transactionDate = formatter.format(new Date(ctx._source.transactionDate));
    """
  }
}
GET transaction_new/_search
</code>
