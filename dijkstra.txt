// start - початкова вершина
// r - максимальна кількість ресурсів, що може взяти з собою мандрівник
// R[i] - додаткова кількість ресурсів у вершині i
// W[i] - вага/довжина найкоротшого шляху start -> i
// H[i] - історія: номер вершини, з якої прийшли до i
// C[i] - решта ресурсів у мандрівника у вершині i
function DijkstraRes1(matrix, start, r, R) {
  const n = matrix.length
  const W = Array(n).fill(Infinity)
  const H = Array(n).fill(-1)
  const C = Array(n).fill(0)
  const visited = Array(n).fill(false)

  W[start] = 0
  C[start] = r

  for (let step = 0; step < n; step++) {
    const i = minWeight(W, visited)
    if (i === -1) break
    visited[i] = true

    for (let j = 0; j < n; j++) {
      if (
        !visited[j] &&
        matrix[i][j] !== 0 &&
        W[i] + matrix[i][j] < W[j] &&
        C[i] >= matrix[i][j]
      ) {
        W[j] = W[i] + matrix[i][j]
        H[j] = i
        C[j] = Math.min(r, C[i] - matrix[i][j] + R[j])
      }
    }
  }

  return { weight: W, history: H }
}

function minWeight(W, visited) {
  let min = Infinity;
  let minIndex = -1;

  for (let j = 0; j < W.length; j++) {
    if (!visited[j] && W[j] <= min) {
      min = W[j];
      minIndex = j;
    }
  }

  return minIndex;
}

function getPath(history, start, end) {
  if (history[end] === -1) return undefined

  const path = [end]
  let current = end
  while (current !== start) {
    current = history[current]
    path.unshift(current)
  }
  return path
}