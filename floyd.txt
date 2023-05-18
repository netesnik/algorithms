// r - максимальна кількість ресурсів, що може взяти з собою мандрівник
// R[i] - додаткова кількість ресурсів у вершині i
// W[i][j] - вага/довжина найкоротшого шляху i -> j
// H[i][j] - історія: зберігає номер наступної вершини на шляху i -> j
// C[i][j] - решта ресурсів у мандрівника у вершині j, якщо починати шлях з вершини i
function FloydRes1(matrix, r, R) {
  const n = matrix.length, W = [], H = [], C = []

  for (let i = 0; i < n; i++) {
    W[i] = []; H[i] = []; C[i] = []
    for (let j = 0; j < n; j++) {
      if (matrix[i][j] === 0 && i === j) {
        W[i][j] = 0
        H[i][j] = -1
        C[i][j] = r
      } else if (matrix[i][j] > 0 && matrix[i][j] <= r) {
        W[i][j] = matrix[i][j]
        H[i][j] = j
        C[i][j] = Math.min(r, r - matrix[i][j] + R[j])
      } else {
        W[i][j] = Infinity
        H[i][j] = -1
        C[i][j] = -1
      }
    }
  }

  for (let k = 0; k < n; k++) {
    for (let i = 0; i < n; i++) {
      for (let j = 0; j < n; j++) {
        if (
          W[i][k] + W[k][j] < W[i][j] &&
          C[i][k] >= W[k][j]
        ) {
          W[i][j] = W[i][k] + W[k][j]
          H[i][j] = H[i][k]
          C[i][j] = Math.min(r, C[i][k] - W[k][j] + R[j])
        }
      }
    }
  }

  return { weight: W, history: H }
}

function getPath(history, start, end) {
  if (history[start][end] === -1) return undefined
  
  let path = [start], current = start
  while (current !== end) {
    current = history[current][end]
    path.push(current)
  }
  return path
}